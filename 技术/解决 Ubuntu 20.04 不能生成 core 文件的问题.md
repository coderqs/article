---
title: 解决 Ubuntu 20.04 不能生成 core 文件的问题
description: 使用 Ubuntu 20.04 测试时发现程序崩溃但没有生成 core 文件，但 `ulimit` 已设为 `unlimited`
author: 清松
date: 2024-03-14T14:14:42+08:00
categories:
  - 技术
tags:
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXH40N1DQJ6EE7NDS6Y3
---
# 解决 Ubuntu 20.04 不能生成 core 文件
## 背景
使用 Ubuntu 20.04 测试时发现程序崩溃但没有生成 core 文件，已检查过 `ulimit -c` 是 `unlimited`

## 原因
是因为 Ubuntu 20.04(貌似 18.04 也有同样的问题) 默认开启了 apport 服务导致的。每次开机 apport 服务都会重置 `/proc/sys/kernel/core_pattern` 文件，这就导致生成的 core 文件被 apport 清理掉了，因此要关闭 apport。

### 关闭 apport
```
sudo systemctl stop apt-daily.timer
sudo systemctl stop apt-daily.service

sudo systemctl stop apt-daily-upgrade.timer
sudo systemctl stop apt-daily-upgrade.service

sudo systemctl disable apt-daily.service
sudo systemctl disable apt-daily.timer
sudo systemctl disable apt-daily-upgrade.timer
sudo systemctl disable apt-daily-upgrade.service

systemctl stop apport.service
systemctl disable apport.service
sed -i 's@enabled=1@enabled=0@g' /etc/default/apport 

sysctl -p
```

## 参考资料
[核心转储，但核心文件不在当前目录中？](https://stackoverflow.com/questions/2065912/core-dumped-but-core-file-is-not-in-the-current-directory)  
[解决Ubuntu重启后，core_pattern失效问题——手动关闭apport](https://www.cnblogs.com/faithfu/p/11933780.html)  