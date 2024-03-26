---
title: MYSQL客户端命令速查
description: ""
author: 清松
date: 2022-08-29T16:38:22+08:00
categories:
  - 工具
tags:
  - MySQL
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACY03NDQY2FBR8K2KP0D2
---
[Mysql常用语句大全](https://zhuanlan.zhihu.com/p/91032512)  
[MySQL常用语句](https://blog.csdn.net/weixin_57109262/article/details/122368376)  
## 遇到的问题
### 在命令行中输入了密码了，执行起来后还要输入密码
`mysql -u root -p passwd` 执行后还是要求输入密码，这是因为 `-p passwd`之间多了个空格，改为 `-ppasswd` 即可。
### Unknown MySQL server host
#### 错误提示如下
```
mysql: [Warning] Using a password on the command line interface can be insecure.
ERROR 2005 (HY000): Unknown MySQL server host '192.168.17.118:20022' (-2)
```
#### 问题原因
mysql 客户端不支持 `mysql -h ip:port` 这样的方式，改为 `mysql -h ip -P port`(注意 `-P` 这里是大写的)。  
#### 参考资料
[ERROR 2005 (HY000): Unknown MySQL server host](https://blog.csdn.net/longxibendi/article/details/6456024)  

### ERROR 1698 (28000)：Access denied for user 'root'@'localhost'
客户端登录到 mysql 上，执行下面的命令
```
use mysql;
select user, plugin from mysql.user;
```
![mysql table]()  
可以看到 root 的默认认证方式是 `auth_socket`，需要将其修改为 `mysql_native_password`，修改命令如下
```
# 5.0.x 的使用
update user set plugin='mysql_native_password' where user='root';

# 8.0.x 的使用
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';

flush privileges;
```
#### 参考资料
[出现ERROR 1698 (28000): Access denied for user ‘root‘@‘localhost‘ 的解决方法](https://blog.csdn.net/weixin_47872288/article/details/122281840)
