---
title: JFrog Artifactory CE 使用 docker 安装
description: 
author: 清松
date: 2023-06-16T09:39:14+08:00
categories:
  - 工具
tags:
  - 制品库
  - Docker
enableMath: true
url: 
draft: false
series:
---
## 使用 docker 安装
### 拉取镜像

#### JFrog Artifactory CE
```
    docker pull docker.bintray.io/jfrog/artifactory-cpp-ce
```
#### 如果想要其他语言的组件
```
 docker pull docker.bintray.io/jfrog/artifactory-oss
```
### 创建数据目录
```
    sudo mkdir -p /opt/jfrog/artifactory/var/etc/
    sudo touch /opt/jfrog/artifactory/var/etc/system.yaml
    sudo useradd -s /bin/false jfrog -u 1030
    sudo chown -R jfrog:jfrog /opt/jfrog/artifactory
```
**注意: 这里的 1030 用户和用户组不能更改，改了的话 docker 容器启动时会报没有权限**

### 启动容器
```
docker run --name artifactory -v /opt/jfrog/artifactory/var/:/var/opt/jfrog/artifactory -d -p 8081:8081 -p 8082:8082 releases-docker.jfrog.io/jfrog/artifactory-cpp-ce:latest
```
### 登录 Artifactory

在浏览器输入 `127.0.0.1:8081` 即可打开页面，默认的用户名和密码:\
User: `admin`\
Password: `password`

## 参考资料

[Artifactory 单节点安装方法](https://www.jfrog.com/confluence/display/JFROG/Installing+Artifactory#InstallingArtifactory-DockerInstallation)\
[JFrog Artifactory CE docker 安装使用记录](https://codeantenna.com/a/WxVpHo0tcG)
