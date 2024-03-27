---
title: 几款 Linux 系统对比
description: ""
author: 清松
date: 2021-07-04T22:54:05+08:00
categories:
  - 杂项
tags:
  - 随记
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACWDHASM199AMZ0JX0GJ2
---
## 前言
因为 Centos 宣布停止发布，改为 Centos stream（相当于从原来的发布顺序`Fedora –> RHEL –> CentOS` 改为 `Fedora –> CentosStream –> RHEL`，缺点就是不如 Centos 稳定了），所以要考虑选择一个替补的系统。

## Debian
Debian 是最古老的基于 Linux 的操作系统之一。它的第一个版本于 1993 年发布。Debian 以其坚如磐石的稳定性和对开源的严格承诺而闻名。它是完全由社区驱动的。

### 发布周期
官方没有明确的发布周期，但基本上是两年出一次。在这里，这些版本以《玩具总动员》系列中的角色命名。例如，最新版本的名称为Buster。

### 稳定性
非常稳定，以稳定性闻名，但其稳定版的软件一般都比较过时。在作为服务器方面非常受欢迎。

### 软件仓库
存储库中只有纯开源软件。

### 性能
附带最少的预装软件，性能上表现更好。

### 安装
使用基于nCurses的Debian 安装程序。具有更多选项，对于新手来说比较棘手

### 目标人群
软件开发人员、游戏玩家、设计师和普通的日常用户。

---
## Ubuntu
Ubuntu 可以说是最受欢迎的基于 Linux 的操作系统。Ubuntu 背后有一家公司 Canonical。开发和支持都由他们完成。一个有趣的事实是 Ubuntu 本身是基于 Debian 的，这就是为什么 Ubuntu 和 Debian 之间很多基本的东西是熟悉的。Ubuntu 开始流行是因为它开始提供令人兴奋的新功能。

### 发布周期
每年发布两个版本，每两年发布一个LTS（长期支持）版本。这些 LTS 版本在发布五年后即将结束。发行版根据模式形容词动物命名（两个词必须以相同的第一个字母开头）。例如，最新版本称为Focal Fossa。

### 稳定性
大部分都是非常稳定的。有时会遇到一些问题，但不是经常遇到。Ubuntu 非常适合一些个人使用和如果系统崩溃不会造成太大损坏的情况。

### 软件仓库
拥有相当庞大的软件存储库。

### 性能
附带了更多的软件，因此性能上表现有些逊色，虽然可以通过卸载来提升性能，但不一定是稳定有效的，因为不知道那些包是系统所依赖的。

### 安装
使用名为Ubiquity的安装程序。

### 目标人群
更偏向于开发人员

## 结论
如果公司的服务器使用更建议使用 Ubuntu TLS，个人服务器可以使用 Debian，个人桌面系统可以使用 Ubuntu。公司服务建议的原因：  
1. 相对门槛低
2. 如果遇见了十分困难的问题可以买技术支持


## 参考资料
[Debian 与 Ubuntu：你需要知道的一切来选择](https://www.fosslinux.com/40109/debian-vs-ubuntu-everything-you-need-to-know-to-choose.htm)    
[Debian 与 Ubuntu：在选择最佳之前需要了解的 15 件事](https://www.ubuntupit.com/debian-vs-ubuntu-top-things-to-know-before-choosing-the-best-one/)   
