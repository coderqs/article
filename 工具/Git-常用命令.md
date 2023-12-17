---
title: "[Git] 常用命令整理"
description: 整理一些 Git 的常用场景的命令，方便速查
author: 清松
date: 2023-12-10T23:42:09+08:00
categories:
  - 工具
tags:
  - Git
math: true
url: 
draft: false
series: Git及衍生品
---
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