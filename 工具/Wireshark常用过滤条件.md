---
title: Wireshark 常用过滤条件
description: ""
author: 清松
date: 2019-01-22T15:17:51+08:00
categories:
  - 工具
tags:
  - Wireshark
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXW3GY6P0GGD6HP3SPZZ
---

Wireshark 在选择网卡的时候设置的过滤器，设置后不可修改，修改需要重新选择网卡  
使用的是BFP语法（Berkeley Packet Filter），一共四个元素：
 - 类型（Type）
   - `host`、`net`、`port`
 - 方向（Dir）
   - `src`、`dst`
 - 协议（Proto）
   - `ether`、`ip`、`tcp`、`udp`、`http`、`ftp`
 - 逻辑运算符
   - `&&`(与)、`||`(或)、`!`(非)

## 常规

### 根据地址
#### 指定 IP
```
ip 192.168.1.1
```
##### 指定源 IP
```
ip.src 192.168.1.1
```
##### 指定目标 IP
```
ip.dst 192.168.1.1
```
#### 指定端口
##### 指定 TCP 协议的端口
```
tcp.prot 8080
```
##### 指定 UDP 协议的端口
```
udp.prot 8080
```
##### 指定源端口(以TCP协议为例)
```
tcp.srcport 8080
```
##### 指定目标端口(以TCP协议为例)
```
tcp.dstport 8080
```
### 根据 mac 地址过滤
```
ether host 00:88:ca:86:f8:0d
ether src host 00:88:ca:86:f8:0d
ether dst host 00:88:ca:86:f8:0d
```
### 根据时间

#### 某一时刻的数据包
```
frame.time == "May 27, 2019 15:23:57.932344000"
```
#### 某一时刻之后的数据包
```
frame.time >= "May 27, 2019 15:23:57.0"
```
#### 某一时间内的的数据包
```
frame.time >= "May 27, 2019 15:23:57.0" && frame.time < "May 27, 2019 15:23:58.0"
```

### 抓取 127.0.0.1 的包
在选择网卡的页面选择 `Adapter for loopback traffic capture`，就可以抓取本地回环的包了(安装 wireshark 时安装了 Microsoft loopback Adapter ，不确定的可以在网络适配器中找有没有这个名字的设备，有就是有安装)。

## 分析 H264 流常用过滤条件
全部的过滤条件可以在官网的[H264 过滤器](https://www.wireshark.org/docs/dfref/h/h264.html) 中查找
### 查找 I 帧
```
h264.nal_unit_type == 5 
```
#### 查找 I 帧的第一个分片
```
h264.nal_unit_type == 5 && h264.slice_type == 7
```
### 查找 SPS/PPS
#### SPS
```
h264.nal_unit_hdr == 7
```
#### PPS
```
h264.nal_unit_hdr == 8
```
### 查找过滤字段的技巧
![](http://tva1.sinaimg.cn/large/ade31767ly1h3y6j3w960j21gm0qle81.jpg)  
1. 选中一个包
2. 在包中找到想要查找的字段，点击
3. 在左下角就会发现这个字段对应的过滤器
## 参考资料

[wireshark 报文分析---根据时间（ArrivalTime）过滤报文](https://blog.csdn.net/ll845876425/article/details/102536822/)    
[wireshark 抓包过滤器使用](https://www.cnblogs.com/laoxiajiadeyun/p/10365073.html)    
[Windows安装Wireshark实现127.0.0.1抓包](https://www.likecs.com/show-205278091.html)    