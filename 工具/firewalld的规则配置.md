---
title: firewalld 的规则配置
description: 
author: 清松
date: 2023-12-10T22:52:08+08:00
categories:
  - 工具
tags: 
math: true
url: 
draft: false
series:
---
## 基本功能
#### 查看防火墙
``` shell
systemctl status firewalld
```

#### 开启防火墙
``` shell
systemctl start firewalld.service
```

#### 重启防火墙
``` shell
firewall-cmd --reload
```

## 端口管理
#### 查看已开放的端口
``` shell
firewall-cmd --zone=public --list-ports
```

#### 添加端口到白名单
``` shell
firewall-cmd --permanent --zone=public --add-port=8484/tcp
```
- `--zone` 作用域
- `--add-port=8484/tcp` 添加端口，格式为：端口/通讯协议
- `--permanent 永久生效`，没有此参数重启后失效

#### 从白名单移除端口
``` shell
firewall-cmd --zone=public --remove-port=8484/tcp
```

## 服务管理
服务只是带有相关名称和描述的端口的简单集合。使用服务比端口更易于管理。

#### 获得可用服务的列表
``` shell
firewall-cmd --get-services
```

#### 查看当前已添加的服务
``` shell
firewall-cmd --zone=public --list-services
```

#### 添加服务
``` shell
firewall-cmd --permanent --zone=public --add-service=ssh
```

### 自定义服务
服务的定义文件通常存放在下面的两个路径中：
1.  `/usr/lib/firewalld/services/`
2.  `/etc/firewalld/services/`

其中第二个路径是存放非标准定义的服务的。上面目录中的文件除去后缀`.xml`就是在 firewalld 的服务列表中显示的服务名称。  
下面展示下 `ssh` 服务的定义文件：

``` xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>SSH</short>
  <description>Secure Shell (SSH) is a protocol for logging into and executing commands on remote machines. It provides secure encrypted communications. If you plan on accessing your machine remotely via SSH over a firewalled interface, enable this option. You need the openssh-server package installed for this option to be useful.</description>
  <port protocol="tcp" port="22"/>
</service>
```

1.  `short` 标签是服务的简称；
2.  `description` 标签是服务的描述，**建议添加描述，以便在需要审核服务时获得更多信息**；
3.  `port` 标签是要开放的端口号和协议，**可以多次指定或者批量**；
``` xml
<!-- 单端口 -->
<port protocol="tcp" port="80"/>

<!-- 多端口 -->
<port protocol="tcp" port="80"/>
<port protocol="tcp" port="8080"/>
... ...

<!-- 批量 -->
<port protocol="tcp" port="10000-12000"/>
```
保存好文件后需要执行一次 `firewall-cmd --reload` 才能识别到

## 参考资料
[如何在 CentOS7 上使用 FirewallD 设置防火墙](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7#getting-familiar-with-the-current-firewall-rules)  
