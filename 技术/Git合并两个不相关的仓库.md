---
title: "[Git] 合并两个不相关的仓库"
description: 将一个不相关的仓库合并到当前仓库中的指定路径，并保留提交的记录
author: 清松
date: 2024-03-20T11:40:09+08:00
categories:
  - 技术
tags:
  - Git
  - Hugo
enableMath: true
url: 
draft: false
series:
---
## 前言
做出这种操作的原因是因为在使用 Hugo 做网站，但是 Hugo 的文章和主题是放在一起的，如果分成两个仓库放，生成的时候就得拷贝过去，这样 git 的提交记录就都没了，就不可以使用 git 的最后提交时间作为文章的最后修改时间了。曾经也试过 submodule 和 sub tree，可 Hugo 不能识别，也达不到效果，最后找来找去才找到合并的方式来解决。

**注意:** 本人的操作是用在 Github Actions 中，其他平台没有进行过验证。
## 步骤
### 说明
假定需要合并的两个仓库分别是 hugo 的主题仓库 hugo-themes 和文章仓库 article，对应的仓库地址分别为 `https://github.com/<xxx>/hugo-themes.git` 和 `https://github.com/<xxx>/article.git`。
操作是将 artcle 合并到 hugo-themes 下的 content/posts/ 目录中

### 实现命令
```
git clone -b main https://github.com/<xxx>/hugo-themes.git .
git remote add article https://github.com/<xxx>/article.git
git fetch article
git merge --strategy=ours  article/main --allow-unrelated-histories -m "GA auto-merge for release"
git read-tree --prefix=content/posts/ -u article/main
```
