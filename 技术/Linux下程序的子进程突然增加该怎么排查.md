---
title: Linux下程序的子进程突然增加该怎么排查
description: 
author: 清松
date: 2023-09-13T15:23:27+08:00
categories:
  - 技术
tags:
  - 问题排查
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXNBC9R5PS6W0F0TE27H
---
## 背景
在测试的时候突然发现程序每做一个特定操作的时候会创建一个子进程，但是这个操作的业务代码中并没有发现有创建进程的动作。估计应该是底层依赖库做的操作，但是不知道会是那个库，进行文本搜索也没有搜索到。  
后来在网上发现了 `strace` 命令，这个命令可以跟踪系统调用，这样跟踪 `fork` 的调用就可以知道是那个库创建的子进程了。

## 使用
1. 先运行要被跟踪的程序
2. 打开终端，在终端输入
   ```
   # 追踪全部的系统调用
   sudo strace -f -o output.txt -p <your_program_pid>
   
   # 只追踪 clone execve 的系统调用
   sudo strace -o output.txt -f -e clone,execve    <your_program>
   ```
   - `-o` 选项是将strace的输出保存到指定的文件中。
   - `-f` 选项可以追踪所有的子进程。
   - `-e` 选项可以指定需要追踪的系统调用。在上面的例子中指定了 clone 和 execve 两个系统调用。
   - `-p` 选项是指定要跟踪的程序的 pid

3. 分析输出文件

**注：`strace` 跟踪期间可能会引起网络拥塞等问题，间接导致丢包（我测试后忘记关了，丢包率达到了90%！）**