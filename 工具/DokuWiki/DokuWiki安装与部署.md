---
title: DokuWiki 的安装与部署
description: 
author: 清松
date: 2019-01-22T15:20:43+08:00
categories:
  - 工具
tags:
  - DokuWiki
enableMath: true
url: 
draft: false
series: DokuWiki
slug: 01HSXACY3CBD9SZHGM9SGQCAQT
---
## 系统要求
1. 支持 PHP 的 Web 服务器  
2. PHP **至少是 5.6 版本**以上的，最好是最新的版本，详细要求可参阅[官网的相关说明](https://www.dokuwiki.org/requirements)

## 下载
在[官网](https://download.dokuwiki.org)下载最新稳定版的压缩包  
![dokuwiki_下载页面](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E6%9C%8D%E5%8A%A1/dokuwiki/dokuwiki_%E4%B8%8B%E8%BD%BD%E9%A1%B5%E9%9D%A2.png)

## Web 服务的配置
在[官网中的 Webserver Specifics](https://www.dokuwiki.org/install)有提供了几款常见的 web 服务器的配置，在这里只展示了我用的 nginx 的配置

### nginx
添加配置文件 `/etc/nginx/conf.d/dokuwiki.conf`
``` txt
server {
    listen 443 ssl;
    server_name wiki.mydomain.com;
    root /var/www/html/dokuwiki;
    index index.html index.php doku.php;

    access_log /var/log/nginx/dokuwiki.access.log;
    error_log /var/log/nginx/dokuwiki.error.log;

    ssl on;
    ssl_certificate /etc/letsencrypt/live/wiki.mydomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/wiki.mydomain.com/privkey.pem;
    ssl_session_timeout 5m;
    ssl_ciphers 'AES128+EECDH:AES128+EDH:!aNULL';
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        try_files $uri $uri/ @dokuwiki;
    }

    location @dokuwiki {
        rewrite ^/_media/(.*) /lib/exe/fetch.php?media=$1 last;
        rewrite ^/_detail/(.*) /lib/exe/detail.php?media=$1 last;
        rewrite ^/_export/([^/]+)/(.*) /doku.php?do=export_$1&id=$2 last;
        rewrite ^/(.*) /doku.php?id=$1 last;
    }

    location ~ /(data|conf|bin|inc)/ {
        deny all;
    }

    location ~* \.(css|js|gif|jpe?g|png)$ {
        expires 1M;
        add_header Pragma public;
        add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
    }

    location ~ /\.ht {
        deny all;
    }
}

server {
    listen 80;
    server_name wiki.mydomain.com;
    add_header Strict-Transport-Security max-age=2592000;
    rewrite ^ https://wiki.mydomain.com$request_uri? permanent;
}
```
**注：要记得修改配置中的证书和密钥的路径**

## php-fpm
将配置文件`/etc/php-fpm.d/www.conf`中的下列项修改一致
```
user = nginx
group = nginx
listen = /var/run/php-fpm/php-fpm.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```
**注：`listen`的配置要与 nginx 配置中的 `fastcgi_pass unix` 一致**

## 启动
执行下面命令启动 php-fpm、nginx 服务
```
systemctl start php-fpm nginx
systemctl enable php-fpm
```
**注：要记得配置防火墙开放对应的端口或者关闭防火墙**

## 部署脚本

### 作用
在 Centos 8 的 dokuwiki 服务的部署脚本(php7.4 + nginx)
### 参数介绍
  * 第一个参数是 dokuwiki 压缩包的下载地址;
  * 第二个参数是证书与服务使用的域名或 ip (如果不填默认使用本机的 ipv4 的 ip);
``` shell
#! /bin/bash
# install php 7.4
yum -y install epel-release
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf -y update
dnf -y install dnf-utils
dnf -y module enable php:remi-7.4
dnf -y install php

# install nginx 
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
dnf -y install nginx

# install dokuwiki
wget $1
tar zxvf dokuwiki*.tgz -C /var/www/html
chown -R nginx /var/www/html
chgrp -R nginx /var/www/html

dokuwiki_config=/etc/nginx/conf.d/dokuwiki.conf
local_ip={$2:-`ifconfig -a|grep inet|grep -v 127.0.0.1|grep -v inet6|awk '{print $2}'|tr -d "addr:"`}
php_sock=/var/run/php-fpm/php-fpm.sock

## Self-signed certificate
mkdir /etc/nginx/ssl && cd /etc/nginx/ssl 
openssl genrsa -des3 -passout pass:123456 -out privkey.pass.key 2048
openssl rsa -in privkey.pass.key -out privkey.key -passin pass:123456
openssl req -new -key privkey.key -out fullchain.csr -subj "/C=CN/ST=Guangdong/L=Guangzhou/O=xdevops/OU=xdevops/CN=$local_ip"
openssl x509 -req -days 365 -in fullchain.csr -signkey privkey.key -out fullchain.crt

cat>"${dokuwiki_config}"<<EOF
server {
    listen 443 ssl;
    server_name $local_ip;
    root /var/www/html/dokuwiki;
    index index.html index.php doku.php;

    access_log /var/log/nginx/dokuwiki.access.log;
    error_log /var/log/nginx/dokuwiki.error.log;

    ssl on;
    ssl_certificate /etc/nginx/ssl/fullchain.crt;
    ssl_certificate_key /etc/nginx/ssl/privkey.key;
    ssl_session_timeout 5m;
    ssl_ciphers 'AES128+EECDH:AES128+EDH:!aNULL';
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        try_files \$uri \$uri/ @dokuwiki;
    }

    location @dokuwiki {
        rewrite ^/_media/(.*) /lib/exe/fetch.php?media=\$1 last;
        rewrite ^/_detail/(.*) /lib/exe/detail.php?media=\$1 last;
        rewrite ^/_export/([^/]+)/(.*) /doku.php?do=export_\$1&id=\$2 last;
        rewrite ^/(.*) /doku.php?id=\$1 last;
    }

    location ~ /(data|conf|bin|inc)/ {
        deny all;
    }

    location ~* \.(css|js|gif|jpe?g|png)\$ {
        expires 1M;
        add_header Pragma public;
        add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }

    location ~ \.php\$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)\$;
        fastcgi_pass unix:$php_sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
    }

    location ~ /\.ht {
        deny all;
    }
}

server {
    listen 80;
    server_name $local_ip;
    add_header Strict-Transport-Security max-age=2592000;
    rewrite ^ https://$local_ip\$request_uri? permanent;
}
EOF

# modify /etc/php-fpm.d/www.conf file
sed -i "/user = apache/cuser = nginx" /etc/php-fpm.d/www.conf
sed -i "/group = apache/cgroup = nginx" /etc/php-fpm.d/www.conf
sed -i "/listen = \/run\/php-fpm\/www.sock/clisten = $php_sock" /etc/php-fpm.d/www.conf
sed -i "/;listen.owner = nobody/clisten.owner = nginx" /etc/php-fpm.d/www.conf
sed -i "/;listen.group = nobody/clisten.group = nginx" /etc/php-fpm.d/www.conf
sed -i "/;listen.mode = 0660/clisten.mode = 0660" /etc/php-fpm.d/www.conf

systemctl start php-fpm nginx
systemctl enable php-fpm

firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload  

# Permanently close SELinux
sed -i "/SELINUX=enforcing/cSELINUX=disabled" /etc/sysconfig/selinux
setenforce 0
```
## 备份
dokuwiki 的备份比较简单，可以只备份以下目录即可
* `data/pages` -包含您当前的页面
* `data/meta` -包含有关您的页面的元信息（例如最初创建者，订阅者等等）
* `data/media` -包含您当前的媒体（图像，PDF等）
* `data/media_meta` -媒体的元数据
* `data/attic` -页面的所有旧版本
* `data/media_attic` -媒体的所有旧版本
* `conf` -配置设置

以上出自官网的[备份教程](https://www.dokuwiki.org/faq:backup)。

## 部署脚本
```
# install php 7.4
yum -y install epel-release
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
rpm -Uvh http://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf -y update
dnf -y install dnf-utils
dnf -y module enable php:remi-7.4
dnf -y install php

# install nginx 
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
dnf -y install nginx

# install dokuwiki
wget $1
tar zxvf dokuwiki*.tgz -C /var/www/html
chown -R nginx /var/www/html
chgrp -R nginx /var/www/html

dokuwiki_config=/etc/nginx/conf.d/dokuwiki.conf
local_ip=`ifconfig -a|grep inet|grep -v 127.0.0.1|grep -v inet6|awk '{print $2}'|tr -d "addr:"`
php_sock=/var/run/php-fpm/php-fpm.sock

## Self-signed certificate
mkdir /etc/nginx/ssl && cd /etc/nginx/ssl 
openssl genrsa -des3 -passout pass:123456 -out privkey.pass.key 2048
openssl rsa -in privkey.pass.key -out privkey.key -passin pass:123456
openssl req -new -key privkey.key -out fullchain.csr -subj "/C=CN/ST=Guangdong/L=Guangzhou/O=xdevops/OU=xdevops/CN=$local_ip"
openssl x509 -req -days 365 -in fullchain.csr -signkey privkey.key -out fullchain.crt

cat>"${dokuwiki_config}"<<EOF
server {
    listen 443 ssl;
    server_name $local_ip;
    root /var/www/html/dokuwiki;
    index index.html index.php doku.php;

    access_log /var/log/nginx/dokuwiki.access.log;
    error_log /var/log/nginx/dokuwiki.error.log;

    ssl on;
    ssl_certificate /etc/nginx/ssl/fullchain.crt;
    ssl_certificate_key /etc/nginx/ssl/privkey.key;
    ssl_session_timeout 5m;
    ssl_ciphers 'AES128+EECDH:AES128+EDH:!aNULL';
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        try_files \$uri \$uri/ @dokuwiki;
    }

    location @dokuwiki {
        rewrite ^/_media/(.*) /lib/exe/fetch.php?media=\$1 last;
        rewrite ^/_detail/(.*) /lib/exe/detail.php?media=\$1 last;
        rewrite ^/_export/([^/]+)/(.*) /doku.php?do=export_\$1&id=\$2 last;
        rewrite ^/(.*) /doku.php?id=\$1 last;
    }

    location ~ /(data|conf|bin|inc)/ {
        deny all;
    }

    location ~* \.(css|js|gif|jpe?g|png)\$ {
        expires 1M;
        add_header Pragma public;
        add_header Cache-Control "public, must-revalidate, proxy-revalidate";
    }

    location ~ \.php\$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)\$;
        fastcgi_pass unix:$php_sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
    }

    location ~ /\.ht {
        deny all;
    }
}

server {
    listen 80;
    server_name $local_ip;
    add_header Strict-Transport-Security max-age=2592000;
    rewrite ^ https://$local_ip\$request_uri? permanent;
}
EOF

# modify /etc/php-fpm.d/www.conf file
sed -i "/user = apache/cuser = nginx" /etc/php-fpm.d/www.conf
sed -i "/group = apache/cgroup = nginx" /etc/php-fpm.d/www.conf
sed -i "/listen = \/run\/php-fpm\/www.sock/clisten = $php_sock" /etc/php-fpm.d/www.conf
sed -i "/;listen.owner = nobody/clisten.owner = nginx" /etc/php-fpm.d/www.conf
sed -i "/;listen.group = nobody/clisten.group = nginx" /etc/php-fpm.d/www.conf
sed -i "/;listen.mode = 0660/clisten.mode = 0660" /etc/php-fpm.d/www.conf

systemctl start php-fpm nginx
systemctl enable php-fpm

firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload  

# 
sed -i "/SELINUX=enforcing/cSELINUX=disabled" /etc/sysconfig/selinux
setenforce 0
```

```
cd /var/www/html/dokuwiki/data/pages
mkdir -p 计算机语言/程序语言
mkdir -p 计算机语言/标记语言
mkdir -p 计算机语言/特定领域语言
mkdir -p 计算机语言/技术
mkdir -p 脚本管理
mkdir -p 工具/编程工具/IDE
mkdir -p 工具/编程工具/版本控制
mkdir -p 工具/编程工具/调试工具
mkdir -p 工具/编程工具/编译工具
mkdir -p 工具/编译器
mkdir -p 工具/虚拟机
mkdir -p 工具/网络工具/抓包
mkdir -p 工具/数据库
mkdir -p 工具/服务/dokuwiki
mkdir -p 工具/服务/hugo
mkdir -p 工具/服务/hexo
mkdir -p 设备/树莓派
mkdir -p 设备/手机
mkdir -p 设备/路由器
mkdir -p 网络/
mkdir -p 网络/科学上网
mkdir -p 网络/协议      
mkdir -p 操作系统/Linux
mkdir -p 操作系统/Windows
mkdir -p 技巧/搜索引擎
mkdir -p 生活/保险

chgrp nginx -R . && chown nginx -R .
```

## 参考资料
[使用 Nginx 的 DokuWiki - 官网](https://www.dokuwiki.org/install:nginx)  
