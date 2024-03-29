---
title: Linux 设置系统时区
description: ""
author: 清松
date: 2021-02-02T14:10:52+08:00
categories:
  - 技术
tags:
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXMS5XWR1EH0BK8PE14M
---
## 方法一：timedatectl
`timedatectl` 是一个命令行工具，它允许你查看并且修改系统时间和日期。它在所有现代的基于 `systemd` 的 Linux 系统中都可以使用。

### 检查时区
执行下面命令：
``` shell
timedatectl
```
执行后会显示系统的时区。我的机器显示如下：
``` text
Local time: Sat 2021-02-19 17:02:22 UTC
           Universal time: Sat 2021-02-19 17:02:22 UTC
                 RTC time: Sat 2021-02-19 17:02:22
                Time zone: UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```
可以看到我的机器的时区 `Time zone: UTC (UTC, +0000)` 是 UTC

### 修改时区
在修改时区时，需要找到想要使用的时区的名字，一般的格式是`地区/城市`。不知道名字的话可以使用 `timedatectl list-timezones` 命令来查看，执行后显示如下：
``` shell
Africa/Abidjan
Africa/Accra
Africa/Addis_Ababa
Africa/Algiers
Africa/Asmara
...
```
找到自己要使用的时区后进行设置，比如我这里使用的是 `Asia/Shanghai`，以
**root** 或 **sudo** 执行下面的命令：
``` shell
sudo timedatectl set-timezone Asia/Shanghai
```
命令执行完成后后再次执行 `timedatectl` 来检查是否设置成功
``` shell
               Local time: Fri 2021-02-19 17:03:47 CST
           Universal time: Fri 2021-02-19 09:03:47 UTC
                 RTC time: Fri 2021-02-19 09:03:47
                Time zone: Asia/Shanghai (CST, +0800)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```
可以看到已经修改成功了。

## 方法二：创建链接
当系统版本较低或`timedatectl` 不可用时，可以通过修改时区的链接文件 `/etc/localtime`，将其链接到 `/usr/share/zoneinfo` 目录下的时区文件来修改时区。

### 查看时区

执行下面命令：
``` shell
date
```
执行后，我的机器显示如下：
``` shell
Fri Feb 19 09:49:20 UTC 2021
```

### 修改时区
在 `/usr/share/zoneinfo` 下找到你想要修改成的时区，然后创建链接文件 `/etc/localtime`。以我使用的 `Asia/Shanghai` 为例，执行下面命令
``` shell
sudo ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
执行完成后使用 `date` 检查是否修改成功，我的机器显示如下：
``` shell
Fri Feb 19 17:50:00 CST 2021
```
可以看到已经修改成功了。

## 参考资料
[如何在 CentOS 8 设置或者修改时区](https://zhuanlan.zhihu.com/p/120861004)   
