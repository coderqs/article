---
title: Linux下scp命令速查
description: ""
author: 清松
date: 2020-10-10T08:58:04+08:00
categories:
  - 技术
tags:
  - 命令
  - Linux
math: true
url: 
draft: false
series:
---
## 拷贝文件夹(包括文件夹本身)
```shell
scp -r $(src_dir)  $(remote_user)@$(remote_ip):$(remote_dst_path)
```
## 拷贝文件夹下的所有文件(不包括文件夹本身)
```shell
scp $(src_dir)/* $(remote_user)@$(remote_ip):$(remote_dst_path)
```
## 对拷文件并重命名
``` shell
scp $(src_file) $(remote_user)@$(remote_ip):$(remote_dst_file)
```
## 将远程服务器上的文件复制到本机
``` shell
scp $(remote_user)@$(remote_ip):$(remote_src_file) $(local_dst_path)
```
## 显示详细的信息
增加参数 `-v`
``` shell
scp -v $(src_file)  $(remote_user)@$(remote_ip):$(remote_dst_path)
```
## 注意
1. 如果远程服务器防火墙有特殊限制，scp便要走特殊端口，具体用什么端口视情况而定，命令格式如下：
``` shell
scp -p 4588 $(remote_user)@$(remote_ip):$(remote_src_file) $(local_dst_path)
```
2. 使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。

## 参考资料
[Linux下scp的用法](https://www.cnblogs.com/qiangqiangqiang/p/7677027.html)
