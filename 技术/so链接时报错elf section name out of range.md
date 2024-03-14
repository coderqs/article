---
title: so链接时报错elf section name out of range
description: ""
author: 清松
date: 2022-06-14T11:27:12+08:00
categories:
  - 技术
tags:
  - 问题排查
math: true
url: 
draft: false
series:
---
## 背景
某个输出的库 \*\*\*.so，同事在依赖这个库的时候链接不过去，提示：
```
/bin/ld: error /home/***/***/***.so : ELF section name out of range
```
没见过这个问题在网上查了下，也没找到有效的解决方法。再一筹莫展的时候同事跟我说有问题的这个库的 ABI 是 unix gnu 和其他的库不一样(unix system v)，突然想起来是不是忘记加 `--no-gun`` 选项了，一查确实是这个原因。

## 解决方法
增加 `--no-gun` 编译选项

## 笔记
库的链接问题除了路径要先瞅瞅库的信息(readelf -h)和其他的有啥不同