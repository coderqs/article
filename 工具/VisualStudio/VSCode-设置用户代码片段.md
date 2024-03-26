---
title: "[VSCode] 设置用户代码片段"
description: 
author: 清松
date: 2021-12-10T22:38:57+08:00
categories:
  - 应用
tags:
  - vscode
enableMath: true
url: 
draft: false
series: VisualStudio
slug: 01HSXACXWN6YYRXP0S1CWED27C
---
## 设置方式
**以 Markdown 为例**  
- `文件 > 首选项 > 用户代码片段` 输入 `markdown`，编辑 `markdown.json`:  
```
{
    // Place your snippets for markdown here. Each snippet is defined under a snippet name and has a prefix, body and 
    // description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
    // $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
    // same ids are connected.
    // Example:
    // "Print to console": {
    //  "prefix": "log",
    //  "body": [
    //      "console.log('$1');",
    //      "$2"
    //  ],
    //  "description": "Log output to console"
    // }
    "blog-header": {
        //"scope": "markdown",
        "prefix": "blog-header",
        "body": [
            "+++  ",
            "author = \"\"  ",
            "title = \"\"  ",
            "date = \"\"  ",
            "description = \"\"  ",
            "tags = [\"\"]  ",
            "categories = [\"\"]",
            "url = \"/blog/  ",
            "[[images]]  ",
            "src = \"/img/  ",
            "alt = \"\"  ",
            "stretch = \"stretchH\"  ",
            "+++  "
        ],
        "description": "博客文件的头，内置标签、分类、标题等信息"
    }
}
```

1.  `文件 > 首选项 > 设置`，设置窗口打开后点击左侧的**文本编辑器(Text Editor)**菜单，在右侧出现的设置界面中下滑，找到并点击 **(Edit in settings.json)** 即可打开 `settings.json`，在其中添加下面的代码:  
```
"[markdown]":  {
    "editor.quickSuggestions": true
    }
```
2.  保存好了以后重启一下 VSCode.  