---
title: vsftpd 安装配置
description: ""
author: 清松
date: 2020-11-05T16:08:47+08:00
categories:
  - 工具
tags:
  - Linux
math: true
url: 
draft: false
series:
---
## 部署脚本
``` shell
#! /bin/bash
yum -y install vsftpd*

sed -i "/anonymous_enable=/canonymous_enable=NO" /etc/vsftpd/vsftpd.conf
sed -i "/chroot_local_user=/cchroot_local_user=YES" /etc/vsftpd/vsftpd.conf
sed -i "/listen=/clisten=YES" /etc/vsftpd/vsftpd.conf
sed -i "/listen_ipv6=/clisten_ipv6=NO" /etc/vsftpd/vsftpd.conf
echo 'allow_writeable_chroot=YES' >> /etc/vsftpd/vsftpd.conf

useradd vsftpd 
echo "123465" | passwd vsftpd --stdin

setsebool allow_ftpd_full_access on

service vsftpd restart
```

## 遇见错误
[vsftpd：500 OOPS: vsftpd: refusing to run with writable root inside chroot ()错误的解决方法](https://blog.csdn.net/bluishglc/article/details/42399439)  
[vsftpd 服务器报错：500 OOPS: vsftpd: refusing to run with writable root inside chroot()](https://blog.51cto.com/kisuntech/1308314)