---
title: 使用 Pandoc 转换文件
description: 使用 Pandoc 将 markdown 文件转换成 dokuwiki 文件
author: 清松
date: 2021-09-15T23:37:00+08:00
categories:
  - 工具
tags:
  - Pandoc
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXZBN7N0CNDABWEY13B5
---

## 简介
提供多种标记语言之间的转换的功能，**但不完全是双向的，部分是单向的**，可转换的关系请参考[此页面](https://www.pandoc.org/index.html)。

## 使用
较短的文字可以使用[在线版](https://pandoc.org/try/)来转换，文字过长就需要[下载](https://www.pandoc.org/installing.html)客户端来转换。

### markdown 转 dokuwiki
```
pandoc -f markdown_github -t dokuwiki in_filename.md -o out_filename.txt
```
**注：markdown 的语法分支比较多，要选对自己使用的是那种 markdown语法。**

## 参考资料
[Pandoc 用户指南](https://www.pandoc.org/MANUAL.html)  
