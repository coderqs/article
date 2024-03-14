---
title: VisualStudio 的一些实用功能
description: ""
author: 清松
date: 2024-01-22T14:45:34+08:00
categories:
  - 工具
tags:
  - VisualStudio
math: true
url: 
draft: false
series:
---
## 代码编辑
### 代码格式化
Visual Studio 2017 15.7 Preview 1 为 C++ 开发人员提供了内置的 [ClangFormat](https://clang.llvm.org/docs/ClangFormat.html) 支持。内置 LLVM、Google、Chromium、Mozilla 或 WebKit 等格式约定。  
### 使用方式
快捷键：`Ctrl+k Ctrl+d`  
菜单选项：`编辑` -\> `格式文档`  

#### 选择格式
`工具` -\> `选项` -\> `文本编辑器` -\> `C/C++` -\> `代码样式` -\> `格式设置` -\> `默认格式设置样式`  
![ClangFormat-Options.png](/工具/visual_studio/ClangFormat-Options.png)

#### 参考资料
[Visual Studio 2017 中的 ClangFormat 支持](https://devblogs.microsoft.com/cppblog/clangformat-support-in-visual-studio-2017-15-7-preview-1/)  
[Visual Studio和VS Code使用clang-format自定义C++代码默认格式化样式](https://blog.csdn.net/xy1157/article/details/93224422)  
