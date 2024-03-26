---
title: 记 undefined reference to 'xxxx' 问题的一种情况
description: ""
author: 清松
date: 2022-03-03T15:17:18+08:00
categories:
  - 技术
  - 工具
  - 应用
  - 方法
  - 杂项
  - 作品
tags: 
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXRPAHGXEHMFYF60ZMQ5
---
## 概述
在重新输出版本的时候修改了下编译选项，结果之前还能正常编译的库突然就编不过去了，还提示了大量的 `undefined reference to` 错误，发现报错的都来自一个库，但库并没有更新过。

## 原因
修改编译选项的时候删掉了地址检查(`-fsanitize=address`)的选项，但依赖库开启了这个选项，所以导致的错误，加回来就好了。

