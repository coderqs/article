---
title: 使用Cloudflare R2 搭建图床
description: ""
author: 清松
date: 2024-03-25T13:45:15+08:00
categories:
  - 工具
tags:
  - 博客
  - CloudFlare
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXVF4Y3QGBZSF3061V7M
---
## 背景
开始搭建个人博客后，文章中的图片怎么存储一直是一个很头疼的问题。之前尝试过挺多方法的，感觉都不是很满意，最近又发现一个新的自建图床、还是免费的，就搭了一个尝试一下，在这里记录下步骤。

## 工具介绍
图床是使用的 Gituhb 上[roimdev](https://github.com/roimdev) 的项目 [roim-picx](https://github.com/roimdev/roim-picx)。将其部署到 Cloudflare Pages 中，图片存储到 Cloudflare R2 中
### Cloudflare R2
Cloudflare R2(以下简称 R2) 是 Cloudflare 推出的存储服务，在我看来和阿里云 OSS 是相同的东西。在这里首先先提醒下，**所谓的免费是有额度的不是无限制的免费**，不过对于一般的个人博客来说是足够使用的，而且就算用超了，价格还是可以接受的([详细定价](https://developers.cloudflare.com/r2/pricing/))

|       | 永久免费      | 超出部分/月费     |
| ----- | --------- | ----------- |
| 存储    | 10 GB/月   | 0.015 美元/GB |
| A 类操作 | 100 万次/月  | 4.50 美元/百万次 |
| B 类操作 | 1000 万次/月 | 0.36 美元/百万次 |
## 步骤
### fork  [roim-picx](https://github.com/roimdev/roim-picx)项目
首先将  [roim-picx](https://github.com/roimdev/roim-picx) fork 到自己的 Github 中。
### 开通 R2 服务
开通需要添加支付信息信用卡或者 PayPal 都可以。这一步就不详细展示了，跟着提示走下去基本就能完成。
### 创建一个存储桶
![[创建r2并设置自定义域_01.png]]![[创建r2并设置自定义域_02.png]]
**桶创建好了是这样**![[创建r2并设置自定义域_03.png]]
4. 给存储桶设置自定义的域名![[创建r2并设置自定义域_04.png]]![[创建r2并设置自定义域_05.png]]![[创建r2并设置自定义域_06.png]]设置好自定义域是这样![[创建r2并设置自定义域_07.png]]
### 创建 KV 并设置
![[创建kv并设置-01.png]]

![[创建kv并设置-02.png]]

![[创建kv并设置-03.png]]

![[创建kv并设置-04.png]]
### 创建 Pages 项目
![[创建pages项目_01.png]]

![[创建pages项目_02.png]]

![[创建pages项目_03.png]]

![[创建pages项目_04.png]]

![[创建pages项目_05.png]]

**注意，这步之后就会开始部署，但是不会成功，需要进入项目的设置中进行如下设置**
![[创建pages项目_06.png]]

### 重试部署
完成上面的设置后，选择重试部署，等待完成即可
![[重试部署.png]]

## 遇到的问题
### 找不到 ./lib/locale/lang/zh-cn
在运行项目的时候报错了，错误的提示是
```
No known conditions for “./lib/locale/lang/zh-cn” entry in “element-plus”
```
这是因为 element-plus 的版本不同导致的，element-plus 版本不同，写法也有所不同。

> 版本：2.2.29

```js
import zhCn from "element-plus/lib/locale/lang/zh-cn";
```

> 版本：2.3.8 （2023-12）

```js
import zhCn from "element-plus/es/locale/lang/zh-cn";
```
修改 `src/App.vue`文件的第 57 行成 2.3.8 版本即可。

参考自: [vue2👉vue3 一些版本问题记录](https://zhuanlan.zhihu.com/p/675006386)

## 参考资料
[白嫖 CloudFlare R2 搭建个人图床](https://testerhome.com/topics/36077)   
[Github roim-picx](https://github.com/roimdev/roim-picx?tab=readme-ov-file)   
[使用R2+Page部署免费的图床【白嫖Cloudflare】](https://blog.lianglianglee.com/2024/01/15/cloudflare-r2-page-image-server/)   
