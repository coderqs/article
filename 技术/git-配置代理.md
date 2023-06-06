---
title: "Git 配置代理"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - Git
  - 代理
categories:
  - 技术
series:
---

在 "科学上网" 后使用 git 克隆 github 上的项目速度还是很慢。后来发现 git clone 时并没有走 "科学上网" 的代理，需要在 git 中设置一下代理才会使用。

## 设置方法
1.  查看 "科学上网" 开放的端口号与协议是什么，我的端口是 1080，协议是 socks。
2.  打开 git bash
  - 只代理 github
``` shell
git config --global http.https://github.com.proxy socks://127.0.0.1:1080
git config --global https.https://github.com.proxy socks://127.0.0.1:1080
```
  - 全局代理
``` shell
git config --global http.proxy socks://127.0.0.1:1080
git config --global https.proxy socks://127.0.0.1:1080
```
**注：**

1.  如果 "科学上网" 使用的协议是 https，则需要将命令中参数  **[socks://127.0.0.1:1080](socks://127.0.0.1:1080)** 替换成 **<https://127.0.0.1:1080>**
2.  socks5 与上同理

## 取消代理
- 取消 github 的代理
``` shell
 git config --global --unset http.https://github.com.proxy
 git config --global --unset https.https://github.com.proxy
``` 
-   取消全局代理
``` shell
git config --global --unset http.proxy
git config --global --unset https.proxy
``` 

## 查看当前设置的代理
``` shell
git config --global --get http.proxy
git config --global --get https.proxy
``` 

## 参考资料
[知乎 - git clone一个github上的仓库，太慢，经常连接失败，但是github官网流畅访问，为什么？@汪小九](https://www.zhihu.com/question/27159393)
