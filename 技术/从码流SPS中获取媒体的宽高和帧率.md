---
title: 从 H264/H265 码流 SPS 中获取媒体的宽高和帧率
description: 
author: 清松
date: 2024-03-26T11:04:10+08:00
categories:
  - 技术
tags:
  - 媒体
  - 转载
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXK2CF29HR8S2J3ER7PZ
---

**转载自** [从H264/H265码流sps中获取宽、高及帧率](https://blog.csdn.net/qq_21743659/article/details/109238267?utm_source=pocket_saves)

---
## 获取媒体的宽高
### H264
公式为
```
Width = (pic_width_in_mbs_minus1+1)*16;  
Height = (pic_height_in_map_units_minus1+1)*16;
```
但以上是针对宽高是 16 的整数倍的情况，当不是 16 整数倍时，`frame_cropping_flag`值为 1，`frame_mbs_only_flag`为 1，公式如下：

```
width = ((pic_width_in_mbs_minus1 +1)*16) - frame_crop_left_offset*2 - frame_crop_right_offset*2;  

height= ((2 - frame_mbs_only_flag)* (pic_height_in_map_units_minus1 +1) * 16) - (frame_crop_top_offset * 2) - (frame_crop_bottom_offset * 2);  
```
比如一个1080P视频的SPS信息如下：
```
pic_width_in_mbs_minus1 : 119
pic_height_in_map_units_minus1 : 67 
frame_mbs_only_flag : 1
mb_adaptive_frame_field_flag : 0
direct_8x8_inference_flag : 1
frame_cropping_flag : 1
   frame_crop_left_offset : 0
   frame_crop_right_offset : 0
   frame_crop_top_offset : 0
   frame_crop_bottom_offset : 4
```
根据第二个公式，带入数值如下
```
width = (119+1) * 16 - 0*2 - 0*2 = 1920
height = (2-1) * (67+1)*16 - 0*2 - 4*2 = 1088 - 8 = 1080
```
以上公式是一年多以前在网上找的，仔细看手册，上面的公式有局限性。根据H264手册Table6-1及7.4.2.1.1，参考mkvtoolnix代码，比如稳妥的计算方法如下：
```
// 宽高计算公式
    width  = (sps->pic_width_in_mbs_minus1+1) * 16;
    height = (2 - sps->frame_mbs_only_flag)* (sps->pic_height_in_map_units_minus1 +1) * 16);
    
    if(sps->frame_cropping_flag)
    {
        unsigned int crop_unit_x;
        unsigned int crop_unit_y;
        if (0 == sps->chroma_format_idc) // monochrome
        {
            crop_unit_x = 1;
            crop_unit_y = 2 - sps->frame_mbs_only_flag;
        }
        else if (1 == sps->chroma_format_idc) // 4:2:0
        {
            crop_unit_x = 2;
            crop_unit_y = 2 * (2 - sps->frame_mbs_only_flag);
        }
        else if (2 == sps->chroma_format_idc) // 4:2:2
        {
            crop_unit_x = 2;
            crop_unit_y = 2 - sps->frame_mbs_only_flag;
        }
        else // 3 == sps.chroma_format_idc   // 4:4:4
        {
            crop_unit_x = 1;
            crop_unit_y = 2 - sps->frame_mbs_only_flag;
        }
        
        width  -= crop_unit_x * (sps->frame_crop_left_offset + sps->frame_crop_right_offset);
        height -= crop_unit_y * (sps->frame_crop_top_offset  + sps->frame_crop_bottom_offset);
    }
```
注：一些 H264 分析工具，比如 CodecVisa 和 H264Visa，1080P 的视频，分辨率为 1920x1088。这是不正确的。此外可以参考 ffmpeg 的 h264_ps.c
```
    if (sps->crop) {
        unsigned int crop_left   = get_ue_golomb(gb);
        unsigned int crop_right  = get_ue_golomb(gb);
        unsigned int crop_top    = get_ue_golomb(gb);
        unsigned int crop_bottom = get_ue_golomb(gb);
        int width  = 16 * sps->mb_width;
        int height = 16 * sps->mb_height;
 
        if (avctx->flags2 & AV_CODEC_FLAG2_IGNORE_CROP) {
            av_log(avctx, AV_LOG_DEBUG, "discarding sps cropping, original "
                                           "values are l:%d r:%d t:%d b:%d\n",
                   crop_left, crop_right, crop_top, crop_bottom);
 
            sps->crop_left   =
            sps->crop_right  =
            sps->crop_top    =
            sps->crop_bottom = 0;
        } else {
            int vsub   = (sps->chroma_format_idc == 1) ? 1 : 0;
            int hsub   = (sps->chroma_format_idc == 1 ||
                          sps->chroma_format_idc == 2) ? 1 : 0;
            int step_x = 1 << hsub;
            int step_y = (2 - sps->frame_mbs_only_flag) << vsub;
 
            if (crop_left  > (unsigned)INT_MAX / 4 / step_x ||
                crop_right > (unsigned)INT_MAX / 4 / step_x ||
                crop_top   > (unsigned)INT_MAX / 4 / step_y ||
                crop_bottom> (unsigned)INT_MAX / 4 / step_y ||
                (crop_left + crop_right ) * step_x >= width ||
                (crop_top  + crop_bottom) * step_y >= height
            ) {
                av_log(avctx, AV_LOG_ERROR, "crop values invalid %d %d %d %d / %d %d\n", crop_left, crop_right, crop_top, crop_bottom, width, height);
                goto fail;
            }
 
            sps->crop_left   = crop_left   * step_x;
            sps->crop_right  = crop_right  * step_x;
            sps->crop_top    = crop_top    * step_y;
            sps->crop_bottom = crop_bottom * step_y;
        }
    } else {
        sps->crop_left   =
        sps->crop_right  =
        sps->crop_top    =
        sps->crop_bottom =
        sps->crop        = 0;
    }
```

图像的宽和高计算：
```
s->coded_width  = 16 * sps->mb_width;
s->coded_height = 16 * sps->mb_height;
s->width        = s->coded_width  - (sps->crop_right + sps->crop_left);
s->height       = s->coded_height - (sps->crop_top   + sps->crop_bottom);
```

### H265
和 H.264 类似，但 SPS 的字段不同了。公式如下：
```
width  = sps->pic_width_in_luma_samples;
height = sps->pic_height_in_luma_samples;
```
当窗口有裁剪时(conformance_window_flag 为 1 )，计算如下：
```
sub_width_c  = ((1==chroma_format_idc)||(2 == chroma_format_idc))&&(0==separate_colour_plane_flag)?2:1;
sub_height_c = (1==chroma_format_idc)&& (0 == separate_colour_plane_flag)?2:1;
width  -= (sub_width_c*conf_win_right_offset + sub_width_c*conf_win_left_offset);
height -= (sub_height_c*conf_win_bottom_offset + sub_height_c*conf_win_top_offset);
```
上式根据H265手册Table6-1及7.4.3.2.1小节计算宽、高。注意，手册里加了1，但实际不使用。参考[mkvtoolnix讨论](https://github.com/mbunkus/mkvtoolnix/issues/1152)
注：对于 1080P 的视频，H265 直接用 `pic_width_in_luma_samples` 及 `pic_height_in_luma_samples` 即得到正确的值。但对于一些奇葩的分辨率，还没有测试过。

## 获取帧率
H264和H265帧率计算公式相同，如下：
```
max_framerate = (float)(sps->vui.vui_time_scale) / (float)(sps->vui.vui_num_units_in_tick);
```
使用 x264 编码 YUV 序列，设置为 25fps 时，`time_scale` 为 50，`num_units_in_tick` 为 1，计算得 50fps，与实际不符。而 x265 用同样的参数编码，计算得到的帧率是正常的。

网上有说法，当 `nuit_field_based_flag` 为 1 时，要再除以 2。另外说 x264 将该值设置为 0，所以得到的值不是实际值。参见：[了解 H.264 num_units_in_tick 和 time_scale](http://forum.doom9.org/showthread.php?t=153019)

