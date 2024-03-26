---
title: Linux下yum针对软件包操作常用命令
description: ""
author: 清松
date: 2020-12-06T23:11:14+08:00
categories:
  - 技术
tags:
  - Linux
  - 命令
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXNWSGGG8ZJ1NSWP51CV
---
## 常用命令
1. 使用YUM查找软件包 
``` 
yum search
```
2. 列出所有可安装的软件包 
``` 
yum list 
```
3. 列出所有可更新的软件包 
``` 
yum list updates
```
4. 列出所有已安装的软件包 
``` 
yum list installed 
```
5. 列出所有已安装但不在 Yum Repository 内的软件包 
``` 
yum list extras 
```
6. 使用YUM获取软件包信息 
``` 
yum info 
```
7. 列出所有可更新的软件包信息 
``` 
yum info updates 
```
8. 列出所有已安装的软件包信息 
``` 
yum info installed 
```
9. 列出所有已安装但不在 Yum Repository 内的软件包信息 
``` 
yum info extras 
```
10. 列出软件包提供哪些文件 
```
yum provides
```

## 参考资料
[CentOS下如何使用yum查看安装过的软件包](https://blog.csdn.net/don_chiang709/article/details/91571424)