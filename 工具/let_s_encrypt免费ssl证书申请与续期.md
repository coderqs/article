---  
title: "Let's Encrypt 免费 SSL 证书申请与续期"  
date: "2021/09/22 23:37"  
description: ""  
slug: ""  
image: ""  
categories:  
    - 工具
tags:  
    - SSL
---  
# Let's Encrypt 免费 SSL 证书申请与续期

[Let's Encrypt](https://letsencrypt.org/)
是一个于2015年三季度推出的数字证书认证机构，旨在以自动化流程消除手动创建和安装证书的复杂流程，并推广使万维网服务器的加密连接无所不在，为安全网站提供免费的传输层安全性协议（TLS）证书。  
Let's Encrypt 由互联网安全研究小组（缩写
ISRG）提供服务。主要赞助商包括电子前哨基金会、Mozilla 基金会、Akamai
以及思科。2015年4月9日，ISRG 与 Linux 基金会宣布合作。  
Let's Encrypt 宣称这一过程将十分简单、自动化并且免费。  

## 安装 certbot

certbot 是 Let's Encrypt 的客户端。我的系统是 Centos，使用的是下面的命令

    yum install -y certbot

使用 `certbot --version` 查看版本显示

    certbot 1.12.0

是此时的最新版。如果不是还可以使用手动安装的方式，在[这里](https://github.com/certbot/certbot/releases)下载最新的发布包解压即可。

## 申请 ssl 证书

申请的命令格式一般是
```
certbot certonly -d "*.youdomain.com" --manual --preferred-challenges dns-01 --server https://acme-v02.api.letsencrypt.org/directory
```
其中：

-   `certonly`，表示使用的插件，Certbot
    有很多插件。不同的插件都可以申请证书，这个可以根据需要自行选择。
-   `-d`，为哪些主机申请证书。如果是通配符，输入
    `*.youdomain.com`（**根据实际情况替换为你自己的域名**）。
-   `–preferred-challenges`，使用 DNS
    方式校验域名所有权。**注：申请通配符证书，只能使用 dns-01 的方式。**
-   `–server`，Let’s Encrypt ACME v2 版本使用的服务器不同于 v1
    版本，需要显示指定

我使用的命令的是
```
certbot certonly -d "*.coderqs.com" --manual --preferred-challenges dns-01 --server https://acme-v02.api.letsencrypt.org/directory
```
输入上述命令后，会有如下提示：
```
    Saving debug log to /var/log/letsencrypt/letsencrypt.log
    Plugins selected: Authenticator manual, Installer None
    Enter email address (used for urgent renewal and security notices)
     (Enter 'c' to cancel): youemail@domain.com

    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    Please read the Terms of Service at
    https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
    agree in order to register with the ACME server. Do you agree?
    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    (Y)es/(N)o: Y

    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    Would you be willing, once your first certificate is successfully issued, to
    share your email address with the Electronic Frontier Foundation, a founding
    partner of the Let's Encrypt project and the non-profit organization that
    develops Certbot? We'd like to send you email about our work encrypting the web,
    EFF news, campaigns, and ways to support digital freedom.
    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    (Y)es/(N)o: Y
    Account registered.
    Requesting a certificate for *.coderqs.com
    Performing the following challenges:
    dns-01 challenge for coderqs.com

    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    Please deploy a DNS TXT record under the name
    _acme-challenge.coderqs.com with the following value:

    5kugLqDPaeD3hEYZl2bHae0VgQoRUt1if39LbuQnr_A

    Before continuing, verify the record is deployed.
    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    Press Enter to Continue
```
上面的邮箱输入你自己常用的，其他的都输入 `Y`
就可以，但是**要注意最后一条，先不要着急按回车**
```
    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    Please deploy a DNS TXT record under the name
    _acme-challenge.coderqs.com with the following value:

    5kugLqDPaeD3hEYZl2bHae0VgQoRUt1if39LbuQnr_A

    Before continuing, verify the record is deployed.
    - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    Press Enter to Continue
```
这里是让你在你的域名管理器中添加一条 TXT
记录来证明这个域名是你的（**如果这里你手快按了回车，一定要再执行一遍命令，因为让你添加的
TXT 的值会改变**，像上面的提示要添加的 TXT
是主机名为`_acme-challenge`，内容为
`5kugLqDPaeD3hEYZl2bHae0VgQoRUt1if39LbuQnr_A` 的记录）。添加完 TXT
后，需要等待其生效后再在这里按回车即可（**certbot
已经自动退出的话可以再执行一遍命令，一般客户端自动退出时候字符串的值是不会变的**）。  
  
**注：等待 TXT 修改生效的时间会比较长，可以使用 \[\[\|nslookup\]\]
来查看 TXT 有没有生效，我使用的命令如下（此命令 windows 与 linux
都支持）**
```
    nslookup -qt=txt _acme-challenge.coderqs.com
```
当其类似如下显示时既是已生效
```
    *** Invalid option: qt=txt
    Server:  UnKnown
    Address:  82.163.142.9

    Non-authoritative answer::
    _acme-challenge.coderqs.com     text =

            "IMdfdsfsJDqBRyRaaEgPPQlEuvtxJQAgWZTIVbLuzDi8U"
```
### 证书续期

Let's Encrypt 的证书有效期是 90 天，允许提前 30
天续签，执行下面命令会续签所有的证书
```
    certbot renew
```
将该命令添加到 crontab 中可以实现自动续签。

## 遇见的问题

### no package certbot available

在 Centos7 上安装时提示`no package certbot available`，原因不明

##### 解决方法
```
    sudo yum -y install yum-utils
    sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    sudo yum-config-manager -y --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional
    sudo yum -y install certbot
```
参考自[CentOS 7: No package certbot available.
#3257](https://stackoverflow.com/questions/53545436/no-package-certbot-available)

### 续签失败

续签失败错误提示：
```
    Could not choose appropriate plugin: The manual plugin is not working; there may be problems with your existing configuration. The error was: PluginError('An authentication script must be provided with --manual-auth-hook when using the manual plugin non-interactively.',). Skipping.
```
原因是申请时没有指定 `--manual-auth-hook` 参数

#### 解决方法

加上 `--manual-auth-hook` 参数重新申请一遍

## 参考资料

[Let's Encrypt -
维基百科](https://en.wikipedia.org/wiki/wiki/Let%27s_Encrypt)  
[Let's
Encrypt免费申请SSL证书](https://jusene.github.io/2018/08/05/letsencrypt/)  
[Nginx启用Let’s Encrypt
SSL证书](https://www.4spaces.org/nginx-lets-encrypt-ssl/)  
[在macOS平台下制作SSL证书，免费域名通配符证、单域名证书、多域名证书教程](https://www.bbsmax.com/A/ZOJPvy8Odv/)  
[Letsencrypt通过DNS
TXT记录来验证域名有效性](https://blog.csdn.net/u012291393/article/details/78768547?utm_source=blogxgwz0)  
