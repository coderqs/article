---
title: is referenced by DSO 解决方法
description: 
author: 清松
date: 2022-03-14T15:12:46+08:00
categories:
  - 技术
tags:
  - GCC
  - C/CPP
  - 问题排查
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXRWWNWGK6XJ6VE41T42
---
## 背景
由我输出了一个动态库(m.so)，其他同事在编译可执行文件(t.out)时链接这个库报的这个错误，大意就是我引用了我依赖的一个动态库(g.a)的符号被隐藏了，但是各个库编译的时候并没有添加选项 `-fvisibility=hidden`，把 t.out 编译时所添加的 g.a 删除掉(t.out 本身不依赖 g.a)后，却又报 m.so 中未定义 xxx 符号(符号是 g.a 中的)，通过 `readelf -a` 查看符号确实未定义。
## 解决方法
调整了我的依赖库的顺序。原因好像是 m.so 其他的依赖库(l.so)也同样使用了 g.a，并且写链接的时候 g.a  写在 l.so 的前面导致的。