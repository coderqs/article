---
title: Nginx 安装与部署
description: ""
author: 清松
date: 2024-01-22T15:00:55+08:00
categories:
  - 工具
tags:
  - Nginx
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXZW2TX2PRCS02QFAHYF
---
## 命令安装
先安装
```
sudo yum install -y yum-utils
```
设置 yum 存储库，创建`/etc/yum.repos.d/nginx.repo`文件输入以下内容：
```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```
默认情况下，使用的是稳定版的 nginx
软件包存储库，如需要使用主线版本，则执行下面命令：
```
sudo yum-config-manager --enable nginx-mainline
```
然后再执行安装命令
```
sudo yum -y install nginx
```

综合以上命令
``` shell
sudo yum install -y yum-utils
cat>"/etc/yum.repos.d/nginx.repo"<<EOF
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
EOF
sudo yum -y install nginx
```
