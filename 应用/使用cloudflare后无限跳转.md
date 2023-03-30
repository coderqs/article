+++
title = "使用 Cloudflare 后网站无限 301 跳转"
description = ""
author = "清松"
tags = ["CloudFlare"]
categories = ["应用"]
[[images]]
  src = "img/"
  alt = ""
  stretch = "stretchH"
+++
# 使用 Cloudflare 后网站无限 301 跳转
## 原因分析
出现这个故障的大部分服务器都是因为服务器端使用了强制 HTTPS，而 CloudFlare 的 Flexible 策略原理是：用户访问时使用 HTTPS 访问到 CF 的节点，然后 CF通过HTTP方式回源到你的服务器去读取数据，这个时候对于你的服务器来说，CF 就是访客，所以服务器返回的状态都是 301。

### 解决方法
将 CloudFlare 的 SSL 策略设为 Full 或者 Full(strict) 就能解决。

### 扩展
#### CloudFlare 的 SSL 可选模式
1.  Off：关闭SSL，全程使用 HTTP；
2.  Flexible：A 使用 HTTPS，B 使用 HTTP，称为灵活加密；
3.  Full：AB全程使用 HTTPS，允许 B 程服务端使用自签名证书；
4.  Full(strict)：全程使用 HTTPS，与 Full 的区别在于服务端必须使用有效的可信任证书；

## 参考资料
[CDN访问异常篇之重定向的次数过多](https://developer.aliyun.com/article/749187)  
[Cloudflare开启HTTPS/SSL后无限301跳转的解决方法](https://vzone.me/897/)  
