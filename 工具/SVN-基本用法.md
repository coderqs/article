---
title: SVN-基本用法
description: ""
author: 清松
date: 2024-03-14T14:25:18+08:00
categories:
  - 工具
tags: 
enableMath: true
url: 
draft: false
series:
---

## 放弃未提交的修改
### 方法一：
删掉修改的文件重新拉取即可
### 方法二：
选中要回退的文件或文件夹 -> 右键 -> `SVN工具选项` -> `revert` -> 勾选要回退的文件 -> 点击 OK

## 代码回滚到之前版本
### 客户端操作
1. 首先用SVN最新代码更新本地代码，保证本地代码和SVN仓库最新的代码一致
2. 然后选中要更新的文件或文件夹，右键调出SVN选项后选择 `update to version` 这里可以点击 `show log` 来找到自己要回退到的修订号或者直接输出修订号，点击 OK
#### 参考资料
[SVN 将代码回滚到之前的版本的方法](https://blog.csdn.net/weixin_48853167/article/details/114869092)
