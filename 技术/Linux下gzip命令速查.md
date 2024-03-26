---
title: Linux 下 gzip 命令速查
description: ""
author: 清松
date: 2020-10-22T17:05:34+08:00
categories:
  - 技术
tags:
  - 命令
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXPZZ0C0HJDRT5A6F8M8
---
## 压缩
### 压缩文件
```
gzip file.gz
```
**注意 压缩为 .gz 文件 源文件会消失**
### 保留源文件
```
gzip -c file.gz > src_file_name
```
### 压缩目录
```
gzip -r dir
```
**注意 gzip 压缩目录 只会压缩目录下的所有文件(每个文件一个压缩包) 不会压缩目录**
## 解压
### 解压文件
```
gzip -d file.gz
gunzip file.gz
```
## 查询
### 查询文件中的内容
```
# zgrep key file.gz
```