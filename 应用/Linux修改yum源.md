---
title: Linux 修改 yum 源
description: 
author: 清松
date: 2020-11-13T15:36:07+08:00
categories:
  - 应用
tags:
  - Linux
math: true
url: 
draft: false
series:
---
# 修改成 163 源
1. 备份默认源
```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```
2. 下载 163 的 yum 源配置文件到系统 yum 配置路径中
```
cd /etc/yum.repos.d/

# 查看系统的版本
cat /etc/redhat-release

# CentOS7：
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo

# CentOS6：
wget http://mirrors.163.com/.help/CentOS6-Base-163.repo

# CentOS5：
wget http://mirrors.163.com/.help/CentOS5-Base-163.repo
```
3. 运行yum makecache生成缓存
```
yum clean all
yum makecache   
yum list 
```
# 阿里源
操作方式与上面一样，只不过是下载的地址不一样
```
# CentOS7
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# CentOS6
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

# CentOS5
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
```

# 本地源
1.挂载CentOS镜像文件
```
mount -t iso9660 /dev/sr0 /opt/centos
```
2.写repo文件并指向镜像的挂载目录
```
vim /etc/yum.repos.d/local.repo

[local]
name=local
baseurl=file:///opt/centos
enabled=1
gpgcheck=0
```
3.清除缓存
```
yum clean all
yum makecache   
yum list   
```

# 修改yum源的优先级
1.查看系统是否安装了优先级的插件
```
rpm -qa | grep yum-plugin-
```
![](https://img2018.cnblogs.com/blog/1047569/201909/1047569-20190920114734451-2023156632.png)

2. 用search查看是否有此插件可用（yum search yum-plugin-priorities），如果没有就安装yum-plugin-priorities.noarch插件
![](https://img2018.cnblogs.com/blog/1047569/201909/1047569-20190920114922169-1907780763.png)

3.查看插件是否启用
```
cat /etc/yum/pluginconf.d/priorities.conf
```
![](https://img2018.cnblogs.com/blog/1047569/201909/1047569-20190920115051693-1664620722.png)

4.修改本地yum源优先使用
```
vi /etc/yum.repos.d/local.repo

priority=1
//在原基础上加入priority=1 ；数字越小优先级越高
//可以继续修改其他源的priority值，经测试仅配置本地源的优先级为priority=1就会优先使用本地源了
```

# 使用epel源
## 安装 epel-release-latest-8.noarch.rpm 的源
```
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```
## 卸载 epel-release-latest-8.noarch.rpm 的源
```
yum remove -y epel-release
```
## 清空epel目录
```
rm -rf /var/cache/yum/x86_64/8/epel/
```
## 遇见的问题
### `Error: Failed to synchronize cache for repo 'epel-modular'`
#### 方案一：
直接删除/etc/yum.repos.d（先备份）
#### 方案二：
yum 成功 但是提示程序不存在无法安装
不删除 /etc/yum.repos.d里面的文件
编辑
```
vi /etc/resolv.conf
```
加上 
```
nameserver 114.114.114.114
```
保存

[epel是什么](https://www.cnblogs.com/fps2tao/p/7580188.html)  
[阿里云 Centos8 报错“Error: Failed to synchronize cache for repo 'epel-modular'”解决方案](https://blog.csdn.net/h2511425100/article/details/104169013)


# 参考资料
[修改CentOS的yum源为国内yum镜像源](http://www.mamicode.com/info-detail-2281451.html)  
[CentOS配置本地yum源/阿里云yum源/163yuan源并配置yum源的优先级](https://www.cnblogs.com/wzhc/p/11556119.html)  
[linux yum 使用epel源](https://www.cnblogs.com/chengege/p/11128650.html)
