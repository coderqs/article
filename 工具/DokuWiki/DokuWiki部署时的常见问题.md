---
title: DokuWiki 部署时的常见问题zheng
description: ""
author: 清松
date: 2019-03-22T15:23:18+08:00
categories:
  - 技术
tags:
  - DokuWiki
  - 故障解决
math: true
url: 
draft: false
series: DokuWiki
---
## 禁止您无权访问此服务器上的/dokuwiki
权限问题，检查以下两点  
1. 注意下dokuwiki下文件的权限（nginx的执行用户是nginx）  
2. dokuwiki.conf的中指定的路径是否正确  

## `because search permissions are missing on a component of the path`
关闭 SELinux 即可，关闭命令
```
setenforce 0
```

## 访问网页返回 `502 bad gateway`
nginx 找不到 php 了，检查下 nginx 对应的服务的配置中的
`fastcgi_pass unix` 项和 php 的 `www.conf` 配置文件中的 `listen`
的值是否一致

## 中文文件名乱码
在网页创建的中文名字空间和页面后，在服务器上全都显示的都是乱码 在文件
`conf/local.php` 最后添加
```
$conf['fnencode'] = 'utf-8';
```
修改 `inc/pageutils.php`
```
function utf8_encodeFN($file,$safe=true){
    global $conf;
    if($conf['fnencode'] == 'utf-8') return $file;

    if($safe && preg_match('#^[a-zA-Z0-9/_\-\.%]+$#',$file)){
        return $file;
    }

    if($conf['fnencode'] == 'safe'){
        return SafeFN::encode($file);
    }

    // 注释掉下面两行
    // $file = urlencode($file);
    // $file = str_replace('%2F','/',$file);
    return $file;
}

function utf8_decodeFN($file){
    global $conf;
    if($conf['fnencode'] == 'utf-8') return $file;

    if($conf['fnencode'] == 'safe'){
        return SafeFN::decode($file);
    }

    // 注释掉下面一行
    // return urldecode($file);
    // 加上下面这行
    return $file;
}
```
参考自：[dokuwiki：解决中文乱码问题](https://www.wangbin.io/blog/it/wiki-dokuwiki-utf8.html)

## PHP 函数xml_parser_create不可用。也许您的托管服务提供商出于某种原因禁用了它
没有安装 xml 模块，搜索安装对应的模块即可
```
    yum install -y php-xml.x86_64
```

## 重定向的次数过多
因为给网站套上了 cdn，其中的 ssl 设置不正确，详细原因和解决方法请参考《[使用cloudflare后无限跳转](/网络/使用cloudflare后无限跳转)》

## dw2pdf 不能导出 pdf
查看 nginx 中 dokuwiki 的日志(`dokuwiki.error.log`)发现了这么一段
```
021/10/13 00:27:31 [error] 30033#30033: *238 FastCGI sent in stderr: "PHP message: PHP Warning:  session_start(): open(/var/lib/php/session/sess_oidq9rhmldgbkn3q39gue3jgck, O_RDWR) failed: Permission denied (13) in /var/www/html/dokuwiki/inc/init.php on line 259PHP message: PHP Warning:  session_start(): Failed to read session data: files (path: /var/lib/php/session) in /var/www/html/dokuwiki/inc/init.php on line 259" while reading response header from upstream, client: 10.160.92.69, server: 192.168.13.97, request: "GET /lib/exe/taskrunner.php?id=wiki%3Adokuwiki&1634056050 HTTP/1.1", upstream: "fastcgi://unix:/var/run/php-fpm/php-fpm.sock:", host: "192.168.13.97", referrer: "https://192.168.13.97/doku.php?id=wiki:dokuwiki"
```
大致就是权限的问题，查看一下发现 `/var/lib/php` 下的所有文件都是属于 root 的，这里应该改成和 web 服务器一样的用户（我使用的是 nginx，这里就改成了 nginx 用户）。
修改完成后再点击导出 pdf 按钮又提示 `mbstring extension must be loaded in order to run mpdf`，这个提示就很明显了，php 缺少 mbstring 扩展库。
使用命令 `php -m | grep mbstirng` 看下果然没有。 
使用命令 `yum install php-mbstring -y` 安装 。
安装完成后重启一下 php-fpm，重启完成后再点击导出 pdf 按钮，就可以了。