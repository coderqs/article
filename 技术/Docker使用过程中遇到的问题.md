---
title: Docker使用过程中遇到的问题
description: ""
author: 清松
date: 2023-05-18T19:22:41+08:00
categories:
  - 技术
tags:
  - Docker
  - 问题排查
math: true
url: 
draft: false
series:
---
## docker run后 logs报/docker-entrypoint.sh: 38: exec: -p: not found
### 背景
启动 nginx 容器时，使用下面的启动命令
```
    docker run -d nginx:latest \
    -p 8080:80 \
    -v /etc/nginx/nginx.conf:/etc/nginx/nginx.conf:ro  \
    -v /etc/nginx/conf.d:/etc/nginx/conf.d:ro  \
    -v /var/log/nginx/:/var/log/nginx \
    -v /var/www/html:/usr/share/nginx/html:ro \
    --name nginx 
```
执行完成后查看容器状态是 `Exited (127)`，这时使用 `docker logs` 提示
```
    /docker-entrypoint.sh: 47: exec: -p: not found
```
### 解决方法
把-d参数放在末尾，即启动命令改为
```
    docker run -p 8080:80 \
    -v /etc/nginx/nginx.conf:/etc/nginx/nginx.conf:ro  \
    -v /etc/nginx/conf.d:/etc/nginx/conf.d:ro  \
    -v /var/log/nginx/:/var/log/nginx \
    -v /var/www/html:/usr/share/nginx/html:ro \
    --name nginx \
    -d nginx:latest 
```
### 参考资料
[docker run后 logs报/docker-entrypoint.sh: 38: exec: -p: not found](https://blog.csdn.net/weixin_43895897/article/details/127944602?spm=1001.2101.3001.6650.3\&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3-127944602-blog-80617448.235%5Ev36%5Epc_relevant_default_base3\&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3-127944602-blog-80617448.235%5Ev36%5Epc_relevant_default_base3\&utm_relevant_index=6)

## 安装docker-compose后没有docker了
### 原因
可能是因为 Docker Compose 安装程序升级了 Docker 版本，并且将旧版本 Docker 删除了。
### 解决方法
将docker清理干净再重新安装
### 参考资料
[安装完docker-compose后，docker没了](http://linuxcpp.0voice.com/?id=38263)\
[Docker与Docker-Compose详解](https://blog.csdn.net/qq_44973159/article/details/121357388)
