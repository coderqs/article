---
title: curl命令常用场景速查
description: 
author: 清松
date: 2022-08-23T16:32:09+08:00
categories:
  - 工具
tags:
  - curl
  - 命令
math: true
url: 
draft: false
series:
---
## 发送 get 请求
默认就会使用 get 请求，直接使用即可
```
curl <url>
```
## 发送 post 请求
```
curl <url> -H <request_header> -X POST -d <post_param>
```
### post 参数是 json
```
curl <url> -H "Content-Type: application/json" -X POST -d <json>
```
**注意:** 看网上的教程有的 `-d` 后面的 json 体是加 `'` (单引号)圈起来即可，但我这里试是不行的，**会把 `'` 一并发出去**，而且 json 体内的**所有 `"` (双引号)都需要转义**(即 `\"`)
### 从文件读取 post 参数
```
curl <url> -H "Content-Type: application/json" -X POST -d @<json_file>
```
### 参考资料
[使用 curl 发送 POST 请求](https://www.cnblogs.com/Aoobruce/p/14804662.html)
[curl发送POST请求](https://www.jianshu.com/p/1f72b475d22d)
