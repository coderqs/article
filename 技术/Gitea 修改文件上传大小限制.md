---
title: Gitea 修改文件上传大小限制
description: ""
author: 清松
date: 2024-03-13T17:37:05+08:00
categories:
  - 技术
tags:
  - Gitea
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXRAP19MR6Y72033C36M
---
## 前言
Gitea默认上传文件大小限制为3MB，所以一些文件文档上传不了
## 解决方法

修改 `app.ini` 文件，添加或者修改  `[repository.upload]`  下 `FILE_MAX_SIZE` 配置项
```
[repository.upload]
; 是否启用存储库文件上传。 默认为true
ENABLED = true
; 上传路径。 默认为`data / tmp / uploads`（tmp在gitea重新启动时被删除）
TEMP_PATH = data/tmp/uploads
; 允许的文件扩展名（.zip），MIME类型（`text/plain`）或通配符类型（image/*`, `audio/*`, `video/*`）的逗号分隔列表。 空值或“ * / *”允许所有类型。
ALLOWED_TYPES =
; 每个文件的最大大小（以MB为单位）。 默认为3MB
FILE_MAX_SIZE = 2048
; 每次上传的最大文件数。 默认为5
MAX_FILES = 5
```
保存重启 Gitea。
## 参考资料
[Gitea修改文件上传大小限制](https://www.lyile.cn/articles/2021/03/04/1614841417314.html)\
[Gitea - 配置说明](https://docs.gitea.io/zh-cn/config-cheat-sheet/#attachment-attachment)
