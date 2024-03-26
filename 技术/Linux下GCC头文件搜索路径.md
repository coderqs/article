---
title: Linux下 gcc 头文件搜索路径
description: 
author: 清松
date: 2021-08-31T17:45:22+08:00
categories:
  - 技术
tags:
  - GCC
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXQ6136099ZGRY25ED1N
---
## 头文件的搜索路径
### 使用 `<>` 引用的头文件
仅查看标准系统目录，搜索顺序如下：
1. `-I` 指定的目录;  
2. gcc 的环境变量 `CPLUS_INCLUDE_PATH`(C 的是 `C_INCLUDE_PATH`);  
3. gcc的内定目录;  
**当多个目录中存在引用的头文件时，会使用最先找到的。**

### 使用 `""` 引用的头文件
优先搜索当前路径，搜索顺序如下：
1. 当前目录;  
2. `-I` 指定的目录;  
3. gcc 的环境变量 `CPLUS_INCLUDE_PATH`(C 的是 `C_INCLUDE_PATH`);  
4. gcc 的内定目录;

存在相同引用的头文件时，处理方式同上。

### gcc 的内定目录
这个内定的目录是根据安装路径决定的(就是编译时参数 `--prefix` 指定的路径)**与 `$PATH` 无关**，默认情况下是:
1. `/usr/include`  
2. `/usr/local/include`  
3. `/usr/lib/gcc/x86_64-redhat-linux/4.1.1/include`  

当设置了安装路径时就是：
1. `$prefix/include`
2. `$prefix/local/include`
3. `$prefix/lib/gcc/$host/$version/include`

其中 `$prefix` 就是安装的路径，`$host` 是编译时参数 `--host` 对应的值，`$version` 是 gcc 的版本。  
这个内定的目录目前还不知道安装后该如何修改，可以通过下面的命令查看：
- C:
``` shell
gcc -xc -E -v -
```
- C++:
``` shell
gcc -xc++ -E -v -
```

## 库文件的搜索路径
### 编译时
1. `-L` 指定的目录;
2. 环境变量 `LIBRARY_PATH`;
3. gcc 内定目录;  
    1. `/lib`  
    2. `/usr/lib`  
    3. `/usr/local/lib`  

**注：库文件的内定目录与头文件的内定目录规则一致。**

### 运行时
1. 编译目标代码时指定的动态库搜索路径(gcc 的参数 `-Wl,-rpath` 设置的值)  
2. 环境变量 `LD_LIBRARY_PATH` 指定的动态库搜索路径  
3. 配置文件 `/etc/ld.so.conf` 中指定的动态库搜索路径  
4. 默认的动态库搜索路径 `/lib`  
5. 默认的动态库搜索路径 `/usr/lib`  

**注意：在 Linux 中动态库搜寻路径并不包括当前文件夹**

## 参考资料
[linux gcc头文件搜索路径](https://blog.csdn.net/andy205214/article/details/77091871)  
[Search-Path](https://gcc.gnu.org/onlinedocs/cpp/Search-Path.html)  
[关于GCC头文件默认搜索路径](https://www.jianshu.com/p/a6d5879ee4e2)  

