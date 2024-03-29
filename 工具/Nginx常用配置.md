---
title: Nginx 常用配置
description: ""
author: 清松
date: 2024-01-22T15:02:13+08:00
categories:
  - 工具
tags:
  - Nginx
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXZPTB4T1ZG2MRXBA77E
---
## Post 返回静态内容
因为 Nginx 有着**无法根据 POST
请求提供静态内容**的限制，所以可以利用这个限制将 405
错误信息替换成静态内容来达到目的。主要配置
```
    error_page 405 =200 $uri
```
### 参考资料
[POST 请求出现 405（不允许）](https://serverfault.com/questions/854425/405-not-allowed-on-post-request)    
[不允许 POST 请求 - 405 Not Allowed -nginx，即使包含标头](https://stackoverflow.com/questions/24415376/post-request-not-allowed-405-not-allowed-nginx-even-with-headers-included)    
[Serving Static Content Via POST From Nginx](http://invalidlogic.com/2011/04/12/serving-static-content-via-post-from-nginx/)    

## 接收 Post 请求参数
在文件 `nginx.conf` 中 `http` 的配置里添加下面的内容  
```
http{
    ...
    log_format post_tracking '$remote_addr - $remote_user [$time_local] "$request" '
    '$status $body_bytes_sent "$http_referer" '
    '"$http_user_agent" "$http_x_forwarded_for" "$request_body" ';
    ...
}
```
### 参考资料
[nginx 配置接收 post 请求参数](https://blog.csdn.net/qq_16142851/article/details/79957532)    
