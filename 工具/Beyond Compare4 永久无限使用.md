---
title: Beyond Compare4 永久无限使用
description: ""
author: 清松
date: 2024-03-25T16:22:48+08:00
categories:
  - 工具
tags:
  - 转载
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACY5PJX02TQZ916E6FQKM
---
今天Beyond Compare4恰巧没法使用了，于是网上搜了一通，总结了如下三种解决方式，分享给大家~

## 方式一

第一种办法（也是最有效的）
删除`C:\Users\用户名\AppData\Roaming\Scooter Software\Beyond Compare 4 `的所有文件，重启Beyond Compare 4即可（**注意：用户名下的AppData文件夹有可能会被隐藏起来**)
## 方式二

删除 `C:\Program Files\Beyond Compare 4\BCUnrar.dll`（安装目录下的BCUnrar.dll文件），这个文件重命名或者直接删除。

## 方式三

修改注册表  
1、在搜索栏中输入 regedit ，打开注册表  
2、删除项目CacheId：
```text
HKEY_CURRENT_USER\Software\Scooter Software\Beyond Compare 4\CacheId
```

以上方法如果还是解决不了，[参考这里](https://u5mwn062nv.feishu.cn/docx/Ewqpd9hDxootrSxEDSkcNbFInqf)。