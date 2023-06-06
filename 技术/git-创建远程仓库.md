---
title: "Git 创建远程仓库"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - Git
categories:
  - 技术
series:
---

# Git 创建远程仓库
## 本地远程仓库
### 创建目录
在本地要当做远程仓库的路径创建一个空目录（与你要本地仓库同名）并创建裸仓库  
``` shell
mkdir ~/Temp/git_server/Test/
git init--bare
```

### 关联仓库
本地仓库关联远程仓库
``` shell
git remote add origin ~/Temp/git_server/Test/
```
如果远程地址设置错了，可以使用以下命令重置
``` shell
git remote set-url origin ~/Temp/git_server/NewTest/
``` 

### 提交
提交到远程仓库
``` shell
git push -u origin master
``` 
<!--
==== 设置自动部署 ====

=== 添加 git hook (设置自动 checkout) ===
在路径 ''.git/hooks/'' 下有许多默认的 hook 脚本，只需要修改 ''post-update.sample'' 即可，下面来根据需求进行操作。
  - 重命名脚本：因为默认.sample结尾的是不会执行的，名字也不能乱改。执行命令 ''mv post-update.sample post-update''
  - 编辑脚本：注释掉默认的操作指令 ''exec git update-server-info''，然后输入我们想要执行的操作命令
<code>
  #!/bin/sh
  #
  # An example hook script to prepare a packed repository for use over
  # dumb transports.
  #
  # To enable this hook, rename this file to "post-update".

  #exec git update-server-info
  # 添加以下三行即可
  unset GIT_DIR
  cd ..
  git checkout -f
</code>
-->
