---
title: Linux设置交换空间
description: ""
author: 清松
date: 2023-08-10T16:14:53+08:00
categories:
  - 技术
tags:
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXN0S36R8WVDD61626NB
---
## 背景
买的 VPS 内存太小只有 1G，不少服务根本跑不起来，因此要设置一下交换空间。VPS 系统是 Debian，这篇笔记就以这个系统为基础。

## 方法
### 查看
刷新交换区
```
swapon -s
```
查看内存
```
free -m
```

### 创建
创建一个4G的交换区文件区块
```
fallocate -l 4G /var/swapfile
```
给文件区块设置权限
```
chmod 600 /var/swapfile
```
设置交换分区
```
mkswap /var/swapfile
```
启动交换分区
```
swapon /var/swapfile
```
刷新交换区查看内存
```
swapon -s
free -m
```
#### 设置开机挂载
```
nano /etc/fstab
```
在文件底部增加如下
```
/var/swapfile swap swap defaults 0 0
```
退出 ctrl+x，是否保存y，确定路径，直接enter

## 参考资料
[debian11设置系统交换区Swap_debian11设置虚拟内存](https://www.rezhuji.com/os/debian11/system/how_to_set_swap_on_debian11.html)    