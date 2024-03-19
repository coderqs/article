---
title: Xshell 和 Xftp 提示“要继续使用此程序，您必须应用最新的更新或使用新版本”
description: ""
author: 清松
date: 2021-12-16T20:58:30+08:00
categories:
  - 技术
tags:
  - 故障解决
enableMath: true
url: 
draft: false
series:
---
## 背景
打开 Xshell 突然间提示 “要继续使用此程序，您必须应用最新的更新或使用新版本”，但是点击确定后又提示已经是最新版，就这么进入了死循环。重启了也不生效，也不想重新安装，因为已有的会话会没掉。因此找了一些解决方法，在此记录下。

## 解决方法
### 方法一：删除 `LiveUpdate.dat` 文件
在 xshell 的安装目录下找到 `LiveUpdate.dat` 文件将其删除，重启 xshell 即可。

**这个方法我这里试是没有成功的**

### 方法二：修改 `nslicense.dll` 文件
1.  使用可以打开 16 进制的编辑器（我是用的 notepad++ 中的插件 Hex-Editor）打开 xshell 的安装目录下的`nslicense.dll` 文件
2.  搜索 `7F 0C 81 F9 80 33 E1 01 0F 86 81`
3.  搜索到后将其中的 `86` 修改为 `83` 并保存文件
4.  重启 xshell 即可

## 参考资料
[Xshell提示更新并且已经是最新版](https://blog.csdn.net/hanhandehanpi/article/details/121392530)
[Xshell 6 提示 “要继续使用此程序,您必须应用最新的更新或使用新版本”](https://vegetable-chicken.blog.csdn.net/article/details/120002352)
