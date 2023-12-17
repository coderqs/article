---
title: CMake 介绍与安装
description: 主要是记录 CMake 在 WIndows 和 linux 下怎么安装
author: 清松
date: 2023-12-10T23:03:14+08:00
categories:
  - 工具
tags:
  - CMake
math: true
url: 
draft: false
series: CMake
---
## CMake 介绍
[CMake](https://cmake.org/)是一个跨平台的开源的元构建系统，可以构建，测试和打包软件。它可以用于支持多个本机构建环境，包括
make，Apple 的 xcode 和 Microsoft Visual Studio。

## CMake 的安装
### Windows
在官网[下载](https://cmake.org/download/)安装包安装即可。

### Centos
#### 安装
``` shell
yum install -y cmake
``` 
安装完成后执行下面命令，检查是否成功。
``` shell
cmake -version
``` 

#### 升级
有时候命令安装的版本不够，需要更高的版本，这时就需要手动进行升级。
- 卸载旧的 cmake  
  ``` shell
  yum remove -y cmake
  ```
- 在官网的下载页面找到需要的 cmake 版本，下载后并解压。我的命令如下:  
  ``` shell
  wget https://github.com/Kitware/CMake/releases/download/v3.20.0-rc2/cmake-3.20.0-rc2-linux-x86_64.tar.gz
  tar -zxvf cmake-3.20.0-rc2-linux-x86_64.tar.gz
  ``` 
- 将其复制到你要安装的目录。我安装的目录是 `/usr/local/`:  
  ``` shell
  cp cmake-3.20.0-rc2-linux-x86_64/* /usr/local/
  ```
- 在 ''/etc/profile '' 的最后一行添加你安装的目录，语法为
  `export PATH=$PATH:你安装的目录/bin`。**我安装的 `/usr/local/`
  目录是系统默认包含的，所以可以省略这步**。
- 检查是否安装成功。

### 可能遇见的问题
#### CMake Error: Error executing cmake::LoadCache(). Aborting
这个是我在手动安装 CMake 的时候没有把 `share/cmake.***` 的文件也拷过去导致的，拷贝过去就可以了。
