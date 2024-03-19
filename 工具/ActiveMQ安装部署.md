---
title: ActiveMQ安装部署
description: 
author: 清松
date: 2022-09-28T18:01:02+08:00
categories:
  - 工具
tags:
  - MQ
  - Linux
  - Ubuntu
enableMath: true
url: 
draft: false
series:
---
## 准备
### JDK
1. 安装 JDK 1.8 以上的版本，可以使用以下命令
```
sudo apt update 
sudo apt install default-jdk 
```

## 安装
### 安装 ActiveMQ
1. 从[官方下载页面](https://activemq.apache.org/components/classic/download/)下载自己要安装的版本，这里以 ActiveMQ 5.17.0 为例，下载并解压
```
wget https://dlcdn.apache.org//activemq/5.17.0/apache-activemq-5.17.0-bin.tar.gz
tar xzf apache-activemq-5.17.0-bin.zip -C /opt
```

2. 默认的 ActiveMQ 仅允许在 localhost 上使用，如果仅是内网使用则可以跳过本步骤。要为本地或公共网络启用 ActiveMQ 访问，需要编辑配置文件 `<activemq_root>/conf/jetty.xml`，搜索以下内容
```
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
         <!-- the default port number for the web console -->
    <property name="host" value="localhost"/>
    <property name="port" value="8161"/>
</bean>
```
将主机值从 localhost 更改为系统 IP 地址或设置 0.0.0.0 以侦听所有接口，保存配置文件

### 启动 ActiveMQ
1. 进入 `<activemq_root>` 目录，执行 `./activemq start` 显示如下即是成功
![]()
3. 访问activemq的管理页面：`http://localhost:8161/admin/`，默认登录用户：admin 密码：admin

## 扩展
### 修改默认登录用户和密码
修改配置文件 `<activemq_root>/conf/jetty-realm.properties` 

### 以服务的方式启动
0. 创建 activemq 用户(因该用户只用于运行activemq，所以创建时关闭登录功能)  
```
useradd -s /bin/false activemq
```
1. 创建文件 `/etc/systemd/system/activemq.service`，添加以下内容
```
[Unit]
Description=Apache ActiveMQ Message Broker
After=network-online.target

[Service]
Type=forking

User=activemq
Group=activemq

Environment="JAVA_HOME=/usr/lib/java/jdk-11"
WorkingDirectory=/opt/apache-activemq-5.17.0/bin
ExecStart=/opt/apache-activemq-5.17.0/bin/activemq start
ExecStop=/opt/apache-activemq-5.17.0/bin/activemq stop
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
**注：其中的`Environment=JAVA_HOME=/usr/lib/java/jdk-11`需要根据自己的实际路径配置**

## 参考资料
[如何在 Ubuntu 22.04 上安装 Apache ActiveMQ](https://tecadmin.net/how-to-install-apache-activemq-on-ubuntu-22-04/)  
[Linux 系统中安装配置Active MQ](https://blog.csdn.net/xingzhe08/article/details/123712035)  
[linux上安装并启动activeMq](https://blog.csdn.net/qq_41793064/article/details/114656001)  