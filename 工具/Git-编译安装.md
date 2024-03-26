---
title: "[Git] Git 的编译安装"
description: ""
author: 清松
date: 2023-12-10T23:32:35+08:00
categories:
  - 工具
tags:
  - Git
enableMath: true
url: 
draft: false
series: Git及衍生品
slug: 01HSXACY22VRSXGT3V16KBHAA2
---
## 准备
### 安装依赖
``` shell
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install -y gcc perl-ExtUtils-MakeMaker
```
### 下载源码
``` shell
git clone https://github.com/git/git.git
```
**注：如果没有给 git [配置代理](/工具/Git-配置代理)的话，建议直接下载压缩包会更快些。**  

## 编译
### 编译命令
``` shell
make prefix=/usr/local/git all 
make prefix=/usr/local/git install
```
这里提醒下，如果 git 的源码是从 windows 下拷过来的要注意下文件的可**执行权限**  

### 创建软连接
``` shell
ln -s  /usr/local/git/bin/git /usr/bin/git
```