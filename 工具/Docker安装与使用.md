---
title: Docker 安装与使用
description: ""
author: 清松
date: 2023-05-11T16:19:05+08:00
categories:
  - 工具
tags:
  - Docker
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACY4D1XBHFK2599H89S4B
---
## 安装

这是[官网提供的在 Ubuntu 上的安装方法](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)  
```
sudo apt-get update
sudo apt-get install -y \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
## 常用命令

### 列出所有镜像
```
    docker images -a
```
### 删除镜像

#### 删除单个镜像
```
    docker rmi <your-image-id>
```
#### 删除多个镜像
```
    docker rmi <your-image-id> <your-image-id> ...
```
### 列出所有容器的数字 ID
```
    docker ps -a -q
```
### 删除容器
```
 docker rm <container-id>
```
### 停止所有容器运行
```
    docker stop $(docker ps -a -q)
```
### 删除所有停止运行的容器
```
    docker rm $(docker ps -a -q)
```
## 参考资料

[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)  
[如何删除 Docker 镜像和容器](https://chinese.freecodecamp.org/news/how-to-remove-images-in-docker/)  
