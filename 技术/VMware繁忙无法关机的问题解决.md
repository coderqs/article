---
title: VMware繁忙无法关机的问题解决方法
description: ""
author: 清松
date: 2019-01-22T15:04:03+08:00
categories:
  - 技术
tags:
  - VMware
  - 故障解决
enableMath: true
url: 
draft: false
series:
---
## 原因
这种问题出现的原因不明（有看到一些说法是 Windows 10 的某些版本和 VMware 兼容性的问题导致的，但也没找到明确的证据证明，都是凭感觉……

### 解决方法
1. 在任务管理器中关闭 `vmware workstation vmx.exe` 进程  
2. 但我在这个地方遇到了问题：就是这个进程根本关不掉！！尝试了各种方法都关不掉！！网上有帖子说[是1903版本与vmware的兼容性](https://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1811242)问题，但也没有给出解决方案。  
3. 通过 Windows 程序与功能将 VMware 修复(没有必要重装，一定不要动原来的虚拟机文件）。修复完成后，虚拟机就可以重新启动了。  

参考自：[Vmware Workstation虚拟机繁忙导致无法关机](https://blog.csdn.net/longmenshenhua/article/details/92230175)
