---
title: Linux下GCC升级
description: 升级 GCC/G++ 的两种方法，一种是通过编译新版本的源码进行升级，另一种是使用 yum 通过 scl 来进行安装新版本方式升级
author: 清松
date: 2020-01-22T14:13:22+08:00
categories:
  - 技术
tags:
  - Linux
  - GCC
math: true
url: 
draft: false
series:
---
## 编译安装 GCC
### 下载源码
#### 方法一
在[gcc官方镜像](https://gcc.gnu.org/mirrors.html)下载源码的压缩包  
``` shell
wget http://ftp.gnu.org/gnu/gcc/gcc-8.4.0/gcc-8.4.0.tar.gz
tar -zxvf gcc-8.4.0.tar.gz
```
#### 方法二
在[github](https://github.com/gcc-mirror/gcc/)上载，**注意这种方式是展开的源码，文件较多较大，下载时间可能更长**
``` Shell
git clone https://github.com/gcc-mirror/gcc.git
```
### 安装依赖
``` shell
yum install -y gmp  gmp-devel  mpfr  mpfr-devel  libmpc  libmpc-devel
```
### 编译安装
``` shell
mkdir gcc-8.4.0-build && cd gcc-8.4.0-build
../gcc-8.4.0/configure --enable-languages=c,c++ --disable-multilib
make -j $(nproc)
```
`configure` 更多的配置选项在[这里查看](https://gcc.gnu.org/install/configure.html)  
**注意**：编译之前要保证 `/tmp` 有足够多的磁盘空间！  
#### 安装
``` shell
make install
```
添加环境变量 编译默认的安装路径是 `/usr/local/bin`, 在 `/etc/profile` 中追加
``` shell
export PATH=/usr/local/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
```
修改完成后保存，使其生效
``` shell
source /etc/profile
```
#### 验证
``` shell
gcc --version
```
输出
``` shell
使用内建 specs。
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/local/libexec/gcc/x86_64-pc-linux-gnu/8.4.0/lto-wrapper
目标：x86_64-pc-linux-gnu
配置为：../gcc-8.4.0/configure --enable-languages=c,c++ --disable-multilib
线程模型：posix
gcc 版本 8.4.0 (GCC) 
```
#### 修改软连接
``` shell
ln -sf /usr/local/lib64/libstdc++.so.6.0.25 /usr/lib64/libstdc++.so.6
ln -sf /usr/local/bin/gcc /usr/bin/cc
```
**注**：如果之前未安装过
gcc，到这里应该就结束了，但之前有就需要将旧的卸载(因为我解决不了两个版本共存的冲突问题)，并链接新的
`libstdc++`
### 遇到的问题
#### Building GCC requires GMP 4.2 , MPFR 2.4.0 and MPC 0.8.0 .
缺少依赖项，使用 
``` shell
yum install -y
```
#### fatal error: Killed signal terminated program cc1plus
原因是内存不够了（vps
内存就512m），可以通过添加虚拟内存来解决，虚拟内存的增加方法可参考《[解决编译GCC内存不足的错误](https://www.cnblogs.com/iakud/p/3825870.html)》和《[设置和修改Linux的swap分区大小](https://www.cnblogs.com/iakud/p/3825848.html)》  

## 通过 yum 升级
[Software Collections](https://www.softwarecollections.org/en/)（也称为SCL）是一个社区项目，使您可以在同一系统上构建，安装和使用多个版本的软件，而不会影响系统默认软件包。通过启用软件集合，您可以访问核心存储库中不可用的较新版本的编程语言和服务。  
SCL 存储库提供了一个名为 Developer Toolset 的程序包，其中包括 GNU
Compiler Collection 的较新版本以及其他开发和调试工具。  
首先，安装 CentOS SCL 发行文件。它是 CentOS Extras
存储库的一部分，可以通过运行以下命令进行安装  
``` shell
yum install -y centos-release-scl
```
当前，可以使用以下开发人员工具集集合：
- Developer Toolset 7  
- Developer Toolset 6  
在此示例中，我们将安装 Developer Toolset 版本 7。运行以下命令：
``` shell
yum install -y devtoolset-7
```
要访问 GCC 版本 7，需要使用 scl 工具启动新的 Shell 实例
``` shell
scl enable devtoolset-7 bash
```
执行完成后查看gcc的版本
``` shell
gcc --version
```
输出
``` shell
gcc (GCC) 7.3.1 20180303 (Red Hat 7.3.1-5)
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
详情可参考[《如何在 CentOS7 上安装GCC编译器》安装多个GCC版本](https://linuxize.com/post/how-to-install-gcc-compiler-on-centos-7/#installing-multiple-gcc-versions)的部分  
