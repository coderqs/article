---
title: V2ray 的安装与配置
description: 使用脚本安装 v2ray 并添加到系统服务
author: 清松
date: 2019-03-11T23:43:30+08:00
categories:
  - 工具
tags:
  - v2ray
  - 科学上网
enableMath: true
url: 
draft: false
---
## V2ray 安装

### 下载安装脚本

```
curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh

curl -O https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh
```

其中 `install-release.sh` 用于安装最新版本的 V2ray 的程序，`install-dat-release.sh` 用于安装geoip.dat和geosite.dat的最新版本。

### 安装 V2ray

- 执行下面命令安装 V2ray
``` shell
bash install-release.sh
```

- 安装的文件结构如下
```
    /usr/local/bin/v2ray
    /usr/local/bin/v2ctl
    /usr/local/share/v2ray/geoip.dat
    /usr/local/share/v2ray/geosite.dat
    /usr/local/etc/v2ray/config.json
    /var/log/v2ray/
    /var/log/v2ray/access.log
    /var/log/v2ray/error.log
    /etc/systemd/system/v2ray.service
    /etc/systemd/system/v2ray@.service
```

- 安装更新 geo 相关的文件
```
    bash install-dat-release.sh
```

### 非官方安装脚本

脚本的安装命令如下：
```
wget -P /root -N --no-check-certificate "https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh" && chmod 700 /root/install.sh && /root/install.sh
```

这个脚本在很多教程中看到过，安装起来确实也方便很多，还支持自动配置常用的几种科学上网方式，其支持的安装类型如下  
- VLESS+TCP+TLS
- VLESS+TCP+xtls-rprx-direct
- VLESS+WS+TLS【支持CDN、IPv6】
- VMess+TCP+TLS
- VMess+WS+TLS【支持CDN、IPv6】
- Trojan
- Trojan-Go+WS【支持CDN、不支持IPv6】

关于脚本的详细内容可以参考其在 Github 上的项目 [v2ray-agent](https://github.com/mack-a/v2ray-agent)。  

## V2ray 配置

可以参考官方在 GitHub 上给出的示例 [v2ray-examples](https://github.com/v2fly/v2ray-examples)。

## 创建为系统服务

在目录 `/usr/lib/systemd/system/` 下创建文件 `v2ray.service`，在其中添加如下内容
```
 [Unit]
 Description=V2Ray - A unified platform for anti-censorship
 Documentation=https://v2ray.com https://guide.v2fly.org
 After=network.target nss-lookup.target
 Wants=network-online.target

 [Service]
 Type=simple
 User=root
 CapabilityBoundingSet=CAP_NET_BIND_SERVICE CAP_NET_RAW
 NoNewPrivileges=yes
 ExecStart=/usr/local/bin/v2ray -config /usr/local/etc/v2ray/config.json
 Restart=on-failure
 RestartPreventExitStatus=23

 [Install]
 WantedBy=multi-user.target
```

注：以上是一个示例，程序的路径与配置文件根据自己实际情况来更改。 [[系统服务文件]]
的编写与说明请参阅对应的页面。