---
title: "Git 的常用命令"
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

# Git 的常用命令
## 基础命令
### 本地
#### 创建存储库
``` shell
git init repositories_name
``` 

#### 状态查看
``` shell
git status
``` 

#### 文件操作
``` shell
# 添加文件
git add filename

# 删除文件
git rm filename

# 移动或重命名
git mv filename_src filename_dst
```
#### 提交文件
``` shell
git commit -m "提交内容的描述"
``` 
#### 比较差异
``` shell
git diff filename
```
#### 查看提交记录
``` shell
git log

# 简版记录
git log --pretty=oneline
```
#### 版本回退
``` shell
# 回退到上个版本
git reset --hard HEAD^

# 回退到上上个版本
git reset --hard HEAD^^

# 回退到上100个版本
git reset --hard HEAD~100

# 回退到指定版本
git reset --hard commit_id
``` 
#### 查看执行命令的记录
``` shell
git reflog
```