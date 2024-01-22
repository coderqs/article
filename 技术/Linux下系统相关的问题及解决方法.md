---
title: Linux 下系统相关的问题及解决方法
description: ""
author: 清松
date: 2019-01-22T15:46:17+08:00
categories:
  - 技术
tags:
  - Linux
  - 故障解决
math: true
url: 
draft: false
series:
---
## 防火墙已开启但没有开放 22 端口，却依然可以使用 ssh 连接
#### 原因
防火墙开放了 ssh 服务，服务把 22
端口开放了。相关的操作可参考防火墙的[firewalld的规则配置#服务管理](/操作系统/Linux/firewalld的规则配置#服务管理)

## ifconfig 获取不到ip
#### 原因
NetworkManager 断开或没有管理网卡设备。使用 `nmcli d status` 可以查看 NetworkManager 状态。添加管理和启动的方法请查看《[CentOS7重启后dhclient未运行，导致IP未获取问题处理](https://support.huaweicloud.com/trouble-ecs/ecs_trouble_0313.html)》  

## systemctl 报错 code=exited status=203
#### 原因
systemctl 执行脚本时需要知道脚本的解释器。
#### 解决方法
脚本启动命令前指定脚本的解释器，可以参考实例记录: [linux脚本自己可以单独执行但在service文件中使用就错误](/操作系统/linux/故障排除/故障记录/linux脚本自己可以单独执行但在service文件中使用就错误)。
