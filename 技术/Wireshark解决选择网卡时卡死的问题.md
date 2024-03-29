---
title: Wireshark 解决选择网卡时卡死的问题
description: 
author: 清松
date: 2019-01-22T15:14:41+08:00
categories:
  - 技术
tags:
  - Wireshark
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXKEF1141D1X3TCE007Y
---
## 背景
在选择网卡后 Wireshark 就会卡死，然后进程未响应，重新打开又重复的卡死无响应的过程。打开任务管理器发现 Wireshark 内存占用迅速上涨，我这里是上涨了 1G 多。  
这个跟 TLS 有关。因为我之前弄 webrtc 的时候配置了 https 的解密，**并且没有还原配置**，**然后我浏览器有很久没有重启**，导致浏览器的`sslkeylogfile` 文件变得很大，而 Wireshark 开始抓包的时候会循环读取`sslkeylogflie` 的内容，所以就出现卡死与崩溃的情况。

### 解决方法
#### 方法一
重启浏览器，删除 `sslkeylogflie` 文件。这是临时解决方法，当 `sslkeylogflie` 文件变大后还是会出现这个问题。

#### 方法二
还原 Wireshark 中关于 TLS 的配置。配置路径 `编辑`-\>`首选项`-\>`Protocols`-\>`TLS` 。

### 参考资料
[《Dive into Windbg系列》Wireshark的卡死与崩溃](https://zhuanlan.zhihu.com/p/33240824)
