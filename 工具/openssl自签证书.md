---  
title: "OpenSSL 自签证书"  
date: "2021/09/15 23:37"  
description: ""  
slug: ""  
image: ""  
categories:  
    - 工具
tags:  
    - SSL 
    - OpenSSL
---  
# OpenSSL 自签证书
## 检查OpenSSL

检查是否已经安装openssl：
```
openssl version
```
一般情况下系统会默认安装。

## 生成自签名的SSL证书和私钥

### 1. 生成私钥
```
openssl genrsa -des3 -out server.pass.key 2048
```
参数解析:

- `genra` 生成RSA私钥
- `-des3` des3算法
- `-out` server.key 生成的私钥文件名
- `2048` 私钥长度

### 2. 去除私钥中的密码
```
openssl rsa -in server.pass.key -out server.key  
```
**注意：有密码的私钥是server.pass.key，没有密码的私钥是server.key**

### 3. 生成CSR(证书签名请求)
```
openssl req -new -key server.key -out server.csr -subj "/C=CN/ST=Guangdong/L=Guangzhou/O=xdevops/OU=xdevops/CN=gitlab.xdevops.cn"
```
-   `req` 生成证书签名请求
-   `-new` 新生成
-   `-key` 私钥文件
-   `-out` 生成的CSR文件
-   `-subj` 生成CSR证书的参数

`subj` 参数说明如下：

| 字段 | 字段含义                | 示例              |
|------|-------------------------|-------------------|
| /C=  | Country 国家            | CN                |
| /ST= | State or Province 省    | Guangdong         |
| /L=  | Location or City 城市   | Guangzhou         |
| /O=  | Organization 组织或企业 | xdevops           |
| /OU= | Organization Unit 部门  | xdevops           |
| /CN= | Common Name 域名或IP    | gitlab.xdevops.cn |

### 4. 生成自签名SSL证书
```
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
-   `-days` 证书有效期
-   `x509` 证书的格式

## 补充

### 1. 从命令行中使用密码生成私钥

使用参数 `-passout`，可以从命令行、文件、标准流输入密码

#### 命令行
```
openssl genrsa -aes128 -passout pass:password 3072
```

#### 文件
```
openssl genrsa -aes128 -passout file:password.txt 3072
```

#### 标准流
```
echo "password" | openssl genrsa -aes128 -passout stdin 3072
```

**注意：`-passout` 参数一定要在参数`-out`和加密位数之前，否则不生效**

> @Mawg: OpenSSL doesn't like it if the -out param comes after the
> 2048 - really, that's supposed to be the last thing on the command
> line (I've updated my answer as such). See the last example, I think
> that's what you want. As for AES-128, someone I trust in these matters
> recommends it over AES-256.

参考自：[How to generate an openSSL key using a passphrase from the
command
line?](https://stackoverflow.com/questions/4294689/how-to-generate-an-openssl-key-using-a-passphrase-from-the-command-line)
