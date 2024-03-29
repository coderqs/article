---
title: C++ 代码风格 —— 注释
description: 
author: 清松
date: 2022-01-22T14:31:39+08:00
categories:
  - 方法
tags:
  - C/CPP
  - 规范设计
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXG85PAB0APKVBAYNR8K
---
平时大家大都只要求代码风格，对注释这部分倒是不怎么看重，一般来说只要注释写清楚了就行。不过个人认为注释如果没有章法的随便写的话反而会对维护代码起到负面作用，因此在这记录三种个人比较认同的注释风格。
## doxygen 风格
指的是 `doxygen` 或者基于 `doxygen` 语法的一些变体的注释。  
这种注释好处是可以直接使用 `doxygen` 的工具生成文档，但坏处就是太啰嗦，而且这种注释大家在修改代码的时候多数都懒得修改，容易造成注释与代码对不上的情况。  

## rustdoc 风格
`rustdoc` 所推荐的注释风格，直接在 `///` 型的注释里使用 `markdown` 语法写注释即可。  
这种注释书写起来很方便，但需要了解 `markdown` 的语法，对经常使用 `markdown` 的人员来说非常直观。

## 简约风格

这种风格提倡在代码中使用良好的命名使代码具有自解释性，以此来适当的减少描述性的注释出现，避免造成代码的不连续。  
这种注释对于命名的能力有着较高要求，毕竟在开发过程中能起一个准确又合适的名字不是一件简单的事情，但是这种风格的代码在看的时候很舒服的。  
使用这种风格有以下几个注意点与建议：  
- 规范、准确的命名。  
- 区分使用代码注释 `/**/` (只注释代码) 与说明注释 `///` ()  
- 可以借助第三方工具生成复杂化注释或者VS自带的snippet,支持自己编辑复杂注释(可以参考[VisualStudio自定义设置](/编程工具/IDE/VisualStudio/自定义设置#Doc Comment))。  
- 减少多行注释，允许的情况下与代码同行。  
- 多人协作最好在注释后加签名和时间，例如：`/// Fix bug #12345 [coderqs@20200101]`  

## 参考资料
[C++注释规范是什么？](https://www.zhihu.com/question/371144076)   