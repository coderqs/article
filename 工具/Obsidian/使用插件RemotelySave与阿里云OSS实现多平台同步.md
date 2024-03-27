---
title: Obsidian 使用 Remotely Save与阿里云 OSS 实现多平台同步
description: 
author: 清松
date: 2023-12-06T09:45:27+08:00
categories:
  - 工具
tags:
  - Obsidian
  - 多平台同步
enableMath: true
url: 
draft: false
series: Obsidian
slug: 01HGVT9S7D91RNDE5HED6XWDNN
slug: 01HSXACXZH3S8ZBPY2G0YFSE3H
---

Remotely Save 是 Obsidian 的一个第三方同步插件。Obsidian 用户和社区开发者发掘了不少官方同步以外的同步方式，不过大多需要在移动端借助第三方工具如 Syncing 或 Folder Sync。这次的 Remotely Save 在移动端不需要第三方工具辅助，仅插件本身即可实现同步，我认为是一个非常好的插件。

Remotely Save 支持 OneDrive、Dropbox、webdav、S3 服务，前两者因为网络原因在国内速度不稳定，网络上已有网友分享腾讯云 COS 的分享，但还没有阿里云 OSS 版本，虽本质相同，但细节有些许出入，本文简要记录一下阿里云 OSS 的配置过程。

**执行任何操作前请做好数据备份**

注：此方法会丢失笔记真实的**最后修改时间**，会改变为最后一次上传到 oss 的时间

## 参考资料
[使用 Remotely Save + 阿里云 OSS 实现多平台同步](https://forum-zh.obsidian.md/t/topic/5362)  
