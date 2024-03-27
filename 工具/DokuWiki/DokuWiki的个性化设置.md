---
title: DokuWiki 的个性化设置
description: ""
author: 清松
date: 2019-03-22T15:27:33+08:00
categories:
  - 工具
tags:
  - DokuWiki
enableMath: true
url: 
draft: false
series: DokuWiki
slug: 01HSXACY2ZEY141TEAE9C08GFD
---
## 添加侧边栏
有的主题是默认带有侧边栏的，而我使用的 Bootstrap3 主题带有右侧边栏，其显示当前文章的内容的索引，想要显示整个 wiki 的索引还需要使用插件[start#IndexMenu](/工具/服务/dokuwiki/插件与主题/start#IndexMenu)。插件的添加与使用请参考其文档，这里介绍如何使其成为独立的侧边栏。
1.  添加一个名为 `sidebar` 的页面。**注：这是默认的名字，如果修改了配置中的 `基本配置`-\>`sidebar` 的配置，需要与其修改后的名字一致**
2.  编辑该页面，添加如下语句，保存即可。
```
  {{indexmenu>..#1|js navbar nocookie}}
```

## 在侧边栏中隐藏 `sidebar` 和 `start` 页面
上面方法添加的侧边栏中会把 `sidebar` 和 `start`
页面显示出来，如果不想显示，则将 `sidebar` 页面中的语句修改成下面的即可
```
  {{indexmenu>..#1 |js#bj-tango.png navbar max#3#2 skipfile+/(^|:)(start|sidebar)$/ skipns=/^users$/}}
```
**注：上面的语句还隐藏了 users 名字空间，如果想显示，则将`skipns=/^users$/` 删掉即可**。

## 修改上传附件大小为2M限制
修改 `/etc/php.ini` 中的两个参数
```
  upload_max_filesize = 10M 
  post_max_size = 10M
```
**注**: Windows环境中尽管会提示上传文件过大，但好像实际并不会限制上传。

## 替换图标
替换 `dokuwiki/lib/tpl/dokuwiki/images/` 路径下的文件：
* `logo.png` 界面的图标  
* `favicon.ico` 浏览器标签上的图标  

## 修改页脚
修改文件 `dokuwiki/lib/tpl/dokuwiki/tpl_footer.php`

## 修改时区
### dokuwiki
修改 `dokuwiki/inc/init.php` 文件中的
```
    date_default_timezone_set('PRC')
```
**注**：`PRC` 标识中国时区

### php
修改 `php.ini` 文件，找到下面部分
```
[Date]
; Defines the default timezone used by the date functions
; http://php.net/date.timezone
;date.timezone =
date.timezone = Asia/Shanghai
```
重启 web 服务即可。  

## 修改上传文件格式的限制
- 系统默认支持类型：修改 `dokuwiki/conf/mime.conf`，在里面增加或注释掉对应的文件格式即可。
- 用户自定义类型：修改 `dokuwiki/conf/mime.local.conf`。

## 使用自定义的邮箱发送邮件
### 使用 php 内置的邮箱功能
1. 勾选`管理` --\> `配置设置` --\> `通知设置` 中 `启用页面订阅支持`
2. 在 `自动发送邮件时使用的邮件地址` 中输入要使用的邮箱，例：email@email.com
3. 也可以增加配置  `自动发送邮件时使用的邮件地址前缀`，使其在所有发送邮件的主题添加这个前缀，方便过滤邮件。

### 使用插件的邮箱功能
上面的配置使用的是 php 的内置邮件功能，当其不可用时可以添加插件 [smtp](/工具/服务/dokuwiki/插件与主题/start#smtp) 来提供邮件功能
1. 添加插件 smtp。
2. 设置 smtp，设置的路径为 `管理` --\> `配置设置` --\> `Smtp`
    1. 在邮箱运营商那里找到相应的 stmp 地址、端口与加密方式，将其分别填入`您的 SMTP 发送服务器`、`您的 SMTP 服务器监听端口`、`您的 SMTP 服务器所用的加密类型`
    2. 在 `如果需要认证，在这里输入您的用户名` 中输入邮箱email@email.com，**注意：此用户名必须与上面的`自动发送邮件时使用的邮件地址` 中的设置的邮箱相同**
    3. `对应上面用户名的密码` 为 password 保存即可。

# 参考资料
[dokuwiki插件的常用配置及其他Tips](https://blog.csdn.net/leekwen/article/details/54907445)    
[DokuWiki 用户手册](https://www.dokuwiki.org/start?id=zh:manual)    
[玩转Dokuwiki](https://blog.csdn.net/dclingcloud/article/details/86727132)    
[DokuWiki相关](https://www.dazhuanlan.com/2019/09/24/5d89567416891/)    
