---
title: 小米路由器 Mini 刷 Openwrt
description: 
author: 清松
date: 2023-12-07T23:27:29+08:00
categories:
  - 应用
tags:
  - openwrt
  - v2ray
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXT65XM2WAJG4FTDR460
---

## 将系统刷成开发版
1.  在 [MiWiFi](http://www1.miwifi.com/miwifi_download.html) 下载路由器对应的 ROM 开发版，版本[2.21.109](http://bigota.miwifi.com/xiaoqiang/rom/r1cm/miwifi_r1cm_firmware_2e9b9_2.21.109.bin)
## 安装 miwifi_ssh
1.  在 [MiWiFi 开发](http://www1.miwifi.com/miwifi_open.html)页面点击 “开启 SSH 工具” 下载 SSH 工具 `miwifi_ssh.bin`
2.  将路由器绑定小米账号后，在页面会显示 root 的密码
3.  将 `miwifi_ssh.bin` 放入 U 盘，删除 U 盘上的其他文件，其他操作与刷 ROM 一致  
## 刷成 OpenWrt
在 OpenWrt 的官网下载对应版本的固件，我的是[小米路由器 MINI](https://openwrt.org/toh/xiaomi/mini)，其他型号的路由器可以在官网的[Table of Hardware: Firmware downloads](https://openwrt.org/toh/views/toh_fwdownload)中搜索下载
```
scp openwrt*.bin  root@192.168.31.1:/tmp
cd  /tmp
mtd -r write openwrt*.bin firmware 
```

如遇以下报错，改 `firmware` 为 `OS1` 即可
```
Could not open mtd device: firmware
Can't open device for writing!
```
## 修改官方源
**注意：不同 CPU 的路由器的 OpenWrt 源的配置不完全相同，需要根据 CPU 的型号来配置，小米路由器 MINI 的 CPU 是 MT7620A**  
源配置文件的路径: `/etc/opkg/distfeeds.conf`，将官方源注释掉后填入下面的源
```
src/gz reboot_core http://mirrors.ustc.edu.cn/openwrt/releases/19.07.5/targets/ramips/mt7620/packages
src/gz reboot_base http://mirrors.ustc.edu.cn/openwrt/releases/19.07.5/packages/mipsel_24kc/base
src/gz reboot_luci http://mirrors.ustc.edu.cn/openwrt/releases/19.07.5/packages/mipsel_24kc/luci
src/gz reboot_packages http://mirrors.ustc.edu.cn/openwrt/releases/19.07.5/packages/mipsel_24kc/packages
src/gz reboot_routing http://mirrors.ustc.edu.cn/openwrt/releases/19.07.5/packages/mipsel_24kc/routing
src/gz reboot_telephony http://mirrors.ustc.edu.cn/openwrt/releases/19.07.5/packages/mipsel_24kc/telephony
```

## openwrt 安装中文语言包
语言的设置选项在路由的管理界面中 System -\> System -\> System Properties -\> Language and Style -\> Language，刚刷完的固件时选项只有英文，其他的需要安装

### 自动安装
在路由的管理页面中 System -\> Software，在 Download and install package 旁边的文本框中输入 `luci-i18n-base-zh-cn` 点击 OK 系统会自动下载并安装

## 参考资料
[OPENWRT安装中文语言包](https://blog.csdn.net/myweishanli/article/details/45331975)  
[OpenWrt之v2ray安装及配置](https://www.zzhyun.com/2020/09/04/178/)  
[小米路由器 mini 刷 OpenWrt/PandoraBox/LEDE](https://leamtrop.com/2017/05/11/flash-openwrt-squashfs/)

------------------------------------------------------------------------

## 额外记录
### 安装 v2ray
下载 [openwrt-v2ray](https://github.com/kuoruan/openwrt-v2ray) 和 [luci-app-v2ray](https://github.com/kuoruan/luci-app-v2ray)
到本地（其中的 luci-i18n-v2ray-zh-cn 是中文包，看需求下载）  
![v2ray-core-mini下载页面](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E8%AE%BE%E5%A4%87/%E8%B7%AF%E7%94%B1%E5%99%A8/v2ray-core-mini%20%E4%B8%8B%E8%BD%BD%E9%A1%B5%E9%9D%A2.PNG)
![luci-app-v2ray下载页面](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E8%AE%BE%E5%A4%87/%E8%B7%AF%E7%94%B1%E5%99%A8/luci-app-v2ray%E4%B8%8B%E8%BD%BD%E9%A1%B5%E9%9D%A2.PNG)  
将下载的文件上传到路由器中，我执行的安装命令是：
```
opkg install v2ray-core-mini_4.34.0-1_mipsel_24kc.ipk 
opkg install luci-app-v2ray_1.5.6_all.ipk
```

### 遇见的问题
#### 1. verify_pkg_installable: Only have 11312kb available on filesystem /overlay, pkg v2ray-core needs 12717 
这是因为 `/overlay` 分区可用空间不够了。  
- 修改`/etc/opkg.conf`，将其中的`overlay_root`的值修改为`/tmp`
```
dest root /
dest ram /tmp
lists_dir ext /var/opkg-lists
option overlay_root /tmp
option check_signature
```
- ~~保存文件后再执行安装命令，在安装命令后增加`-d ram`的参数~~
```
opkg install [package] -d ram
```
~~详情可参见[这里](https://forum.openwrt.org/t/opkg-to-ram-how-to/31172)~~。 但**这种方法行不通**。 -d ram 的选项意思是将软件安装到 `/tmp` 路径下，而 `/tmp` 则是 `tmpfs` 类型的文件系统（这个可以使用 `df -h` 可以看到），是存在内存中的，关机之后就没有了。

#### 2. satisfy_dependencies_for: Cannot satisfy the following dependencies
查看内核版本的命令 `opkg info kernel`
> 快照版本在安装空隙时是否使用官方源有一定的机率出行该问题。
> 解决的方案是编译阶段就将对应的依赖包安装进去。v2ray并不依赖内核版本
>
> 可以尝试手动安装ip，ipset，iptables-mod-tproxy，dnsmasq-full，resolveip
>
> 然后再安装luci-app-v2ray
```
opkg remove dnsmasq
opkg install ip ipset iptables-mod-tproxy dnsmasq-full resolveip
opkg install luci-app-v2ray
```

#### 3. \[info\] Service disabled: main \[info\] Transparent proxy disabled.
```
[info] Service disabled: main 
[info] Transparent proxy disabled.
```
这时是已经安装成功了，这提示中的配置可以在 `/etc/config/v2ray` 中修改。

#### 4. module 'luci.cbi' not found
有说需要[安装`luci-compat`的`luci-ap-sqm`可以解决](https://github.com/kuoruan/luci-app-v2ray/issues/42)，但是使用
opkg 安装提示
```
    Unknown package 'luci-compat'.
    Collected errors:

    * opkg_install_cmd: Cannot install package luci-compat.
```
[解决方法](https://blog.csdn.net/weixin_43274097/article/details/107197717)
```
opkg update
opkg install luci luci-base luci-compat
```
#### 5.
```
    Collected errors:
     * satisfy_dependencies_for: Cannot satisfy the following dependencies for v2ray-core:
     *  ca-certificates
     * opkg_install_cmd: Cannot install package v2ray-core.
```
执行 `opkg update` 后再安装一下

#### 参考资料
[编译Openwrt固件安装软件内核版本不一致问题解决](https://www.haiyun.me/archives/1075.html)   
[OpenWRT内核不符合依赖](https://github.com/kuoruan/luci-app-v2ray/issues/116)   
