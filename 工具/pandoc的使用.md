---
title: "Pandoc 的使用"
description: ""
date: "2021-09-15 23:37:00"
draft: false
authors:
  - "清松"
tags:
  - Pandoc
categories:
  - 工具
series:
---


# Pandoc 的使用
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
