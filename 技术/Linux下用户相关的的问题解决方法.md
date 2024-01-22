---
title: Linux下用户相关的的问题解决方法
description: ""
author: 清松
date: 2019-01-22T15:47:40+08:00
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
## 登录用户出现`-bash-4.2$`

### 原因
用户家目录下的环境变量文件（`.bash_profile` 和 `.bashrc`）丢失了
### 解决方法
从主默认文件/etc/skel/下重新拷贝一份配置信息到此用户的家目录下，命令如下
```
cp /etc/skel/.bashrc  /home/user/
cp /etc/skel/.bash_profile   /home/user
```
拷贝完后注销并重新登录用户即可。
