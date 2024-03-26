---
title: SVN 建立文件夹链接
description: 
author: 清松
date: 2021-12-29T18:14:44+08:00
categories:
  - 工具
tags:
  - SVN
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXYMS6QHR5RJ03V24QAE
---
这个功能和 git 的 submodle 类似，是将其他的外部仓库在自己的仓库里建立一个映射关系，当外部仓库更新后在自己的仓库更新一下就可以了，不用到处复制文件来维护多个版本的文件。
## 添加方法
1. 在自己的仓库中右键选择 `TortoiseSVN ` -> `Properties`  
2. 在弹出的窗口中选择 `new` -> `externals`  
3. 在弹出的窗口中选择 `new` 会再弹出一个窗口
4. 其中 `Local path` 添加的是外部仓库在自己仓库中的名字，`URL` 是外部仓库的 svn 地址，填写完成后点击 `OK`
5. 在自己的仓库中 `Update` 一下外部仓库就添加进来了。
## 说明
1. 在自己仓库中修改外部仓库的内容是无法提交的，需要进入外部仓库在本地仓库的文件夹中才可以提交
2. 在自己仓库中使用 `Update` 也会同时更新外部仓库
## 参考资料
[使用svn:externals建立SVN文件(夹)链接](https://blog.csdn.net/aosica321/article/details/51165618)  
