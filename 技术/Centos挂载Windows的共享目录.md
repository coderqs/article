---
title: Centos挂载Windows的共享目录
description: 在 Centos 挂载上 Windows 的共享目录
author: 清松
date: 2020-01-22T14:19:48+08:00
categories:
  - 技术
tags:
  - Linux
math: true
url: 
draft: false
series:
---
## 挂载步骤
### 1. 在windows上创建一个共享目录
#### 设置共享
右键需要共享的文件夹，点击“共享”，点击“共享此文件夹”，此时可以设置权限。默认权限是“读取”。  
#### 更改权限
接着上述步骤，点击“权限”按钮，打开权限对话框，若需要写权限（修改权限），选中Everyone用户组，选中“修改”复选框或者“完全控制”复选框。点击“应用”“确定”。  
进入“安全”选项卡，选中Everyone用户组，选中“完全控制”，“应用”“确定”。
更改权限的两步必须同时设置才能生效。  
### 2. 在Centos7上挂载共享目录\*\* 执行下面命令
``` shell
mount -t cifs -o username=share,password=share //192.168.31.189/share /share
```
其中:
- `username,password` 是 windows 登录用户名密码  
- `//192.168.31.189/share` 就是windows要的共享文件夹  
- `/share` 是希望 Centos7 将共享文件夹要挂载到的地方，可任意定位置  
### 3. 开机启动就挂载文件夹
在 `/etc/fstab` 文件中添加下列代码即可  
``` shell
//192.168.31.189/share /share cifs username=share,password=share 0 0
```
## 遇到的问题
``` shell
mount -t cifs -o username=share,password=share //192.168.31.189/share /share
mount: //192.168.31.189/share 写保护，将以只读方式挂载
mount: 无法以只读方式挂载 //192.168.31.189/share
```
是因为缺少组件，安装后解决问题（具体是怎么查出来的我也不会😅，《[CentOS7挂载windows共享时提示写保护](http://www.voidcn.com/article/p-fkgygbkn-bdw.html)》中没有解释过程）
``` shell
yum install cifs-utils
```
## 参考
[Windows共享文件夹权限设置](https://blog.csdn.net/kingzone_2008/article/details/8677166)  
[CentOS7挂载Windows下的共享文件夹](https://blog.csdn.net/wm6752062/article/details/80724520)  
[CentOS7挂载windows共享时提示写保护](http://www.voidcn.com/article/p-fkgygbkn-bdw.html)  
