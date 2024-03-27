---
title: Mailx 安装与使用
description: 主要是介绍在 Linux 下如何配置和使用 Mailx 发送邮件
author: 清松
date: 2023-12-10T23:29:01+08:00
categories:
  - 技术
tags:
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACY0KGXACWM2ATY5D6PJX
---
## Mailx 安装
### 安装
执行命令
``` shell
sudo yum install -y mailx
```

### 配置
在配置文件 `/etc/mail.rc` 后面追加
``` shell
 set from=username@domain # 对方收到邮件时显示的发件人

 set smtp=smtp.domain.com # 指定第三方发送邮件的smtp服务器地址

 set smtp-auth-user=username@domain # 第三方发邮件的用户名

 set smtp-auth-password="password" # 用户名对应密码或授权码

 set smtp-auth=login # SMTP的认证方式。默认是LOGIN，也可改为CRAM-MD5或PLAIN方式
```

## Mailx的使用
可以使用 `mail -h` 查看帮助
``` shell
mail: option requires an argument -- h
Usage: mail -eiIUdEFntBDNHRVv~ -T FILE -u USER -h hops -r address -s SUBJECT -a FILE -q FILE -f FILE -A ACCOUNT -b USERS -cUSERS -S OPTION users
```

### 无正文的邮件
``` shell
mail -s "subject" qs@coderqs.com
```
**注：不知道是我的版本的问题还是其他原因，我使用这种方式是发不出去邮件的，执行后一直没有响应，程序像是挂起了一样，既不发送也不结束。**

### 有正文的邮件
#### 从文件中导入正文
``` shell
mail -s "subject" qs@coderqs.com < message_body.txt
```
或者
``` shell
cat message_body.txt | mail -s "subject" qs@coderqs.com 
```

#### 从命令行导入正文
``` shell
echo "massage body content" |  mail -s "subject" qs@coderqs.com
```

### 带附件的邮件
``` shell
mail -s "subject" qs@coderqs.com -a attachment.tar.gz
```

## 参考资料
[Linux发邮件之mail命令详解](https://www.jb51.net/article/100630.htm)    
[mail 参数详解](https://blog.csdn.net/cioujie8131/article/details/100350985)   
