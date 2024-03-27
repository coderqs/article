---
title: NDK 编译出现的错误
description: Undefined reference to 'std::__ndk1::locale::~locale()'
author: 清松
date: 2021-09-07T15:18:59+08:00
categories:
  - 技术
tags:
  - 问题排查
  - Android
  - C/CPP
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXMMHSYZJ5GEFZC1C5TW
---
## `Undefined reference to 'std::__ndk1::locale::~locale()'`
### 原因 
使用的 stl 版本不一致，从这条错误往上找可以看到是那个库使用了的版本不同。
### 解决方法
将 `Application.mk` 中的 `APP_STL := c++_shared` 替换为 `APP_STL := gnustl_static`，删除缓存重新编译。
### 参考资料
[Error Undefined reference to 'std::__ndk1::locale::~locale()'](https://stackoverflow.com/questions/49183886/error-undefined-reference-to-std-ndk1localelocale)     
[Android NDK 链接器错误：错误：对 std::basic_string 的未定义引用](https://stackoverflow.com/questions/38274943/android-ndk-linker-errorerror-undefined-reference-to-stdbasic-string)    