---
title: CPP在注释中遇到意外的文件结束
description: ""
author: 清松
date: 2024-01-22T17:38:06+08:00
categories:
  - 技术
tags:
  - C/CPP
enableMath: true
url: 
draft: false
series:
---
官方给出的错误原因是缺少注释终结器 (`*/`)，实际查找并未找到缺少`*/`的错误。

utf8 格式出错，有一个注释是`/* 中文*/`，这里由于编码问题，中文和英文联合起来，吞掉了注释的`*/`，导致 bug。只需要加个空格改为 `/* 中文 */`。

**所以为了不出错，中文注释可能应该前后加英文字符，如前面加空格，后面加‘.’号。**

[“在注释中遇到意外的文件结束”--记一个令人崩溃的bug](https://www.cnblogs.com/huipengly/p/10473288.html)