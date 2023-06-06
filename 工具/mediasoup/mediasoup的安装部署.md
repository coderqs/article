---
title: "[Mediasoup]安装与部署"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - Mediasoup
categories:
  - 工具
series:
---

# Mediasoup 的安装与部署
## 环境要求
官方要求：
* cmake \>= 3.5
* gcc 和 g++ \>= 4.9 或 clang（具有C++ 11支持）
* node.js \>= 10.0, npm \>= 6.3

## 下载
需要下载 [mediasoup v3](https://github.com/versatica/mediasoup)
[mediasoup v3 demo github](https://github.com/versatica/mediasoup-demo)，在 github 上下载就可以  

## 安装
### 安装 mediasoup-demo
``` shell
cd mediasoup-demo/server
npm install
cd ../app
npm install
```
**注意**： 如果 npm 速度过慢或者超时，可以将源修改为淘宝的源或者使用 `cnpm`，`cnpm` 的安装方法可以[参考这里](https://www.jianshu.com/p/115594f64b41)  

#### 配置 server
```
cd server
cp config.example.js config.js
```
因为新版本demo没有生成了证书，所以需要[自行生成证书](/工具/其他/openssl自签证书)并且放置相对应的目录(以 server 目录为根的相对路径，如果想使用绝对路径则需要将配置项中的 `${__ dirname}` 删掉).  

#### 修改 config.js 文件
```
       ...
         tls        :
         {
             //需要生成一个证书秘钥，这个用命令自行生成，并放置相对应的目录
             cert : process.env.HTTPS_CERT_FULLCHAIN || `${__dirname}/certs/xxx_server.crt`,
            key  : process.env.HTTPS_CERT_PRIVKEY || `${__dirname}/certs/xxx_server.key`
        }
    },
    ...
    webRtcTransportOptions :
        {
            listenIps :
            [
                {
                    ip          : process.env.MEDIASOUP_LISTEN_IP || '192.168.137.139', //修改为服务端的IP地址，如果是公网则为外网 IP
                    announcedIp : process.env.MEDIASOUP_ANNOUNCED_IP
                }   
            ],
    ...
        plainRtpTransportOptions :
        {
            listenIp :
            {
                ip          : process.env.MEDIASOUP_LISTEN_IP || '1.2.3.4',
                announcedIp : process.env.MEDIASOUP_ANNOUNCED_IP
            },
            maxSctpMessageSize : 262144
        }
    }
};
```
**注意事项**：  
- 确保将 `https.listenIp` 设置为 `0.0.0.0`;  
- 确保 TLS 证书位于 `server/certs` 名称为 `fullchain.pem` 和的目录中 `privkey.pem`;  
- 默认的 mediasoup 端口范围是 2000-2020，不适合生产。应该使用更高的端口，但是然后应该在 `network="host"`模式下运行容器;  

#### 启动服务
```
cd server
DEBUG="*mediasoup* *ERROR* *WARN*" INTERACTIVE="true"  node server.js
```

#### 启动客户端
```
cd app
npm start
```

#### 在浏览器打开
访问 `https://192.168.11.18:3000/?info=true`  
**注意**：
1. 防火墙是否关闭或允许访问 3000 端口;  
2. URL中的 `/?info=true` 一定要带;  

#### 邀请参会
在 web 页面的上方中间，有一个 `INVITAIONLINK` 的按钮，点击后会复制邀请链接，当通过这个邀请链接进入 web 页面时就会进行参考。  

## 遇见的问题
### OSError: \[Errno 2\] No such file or directory
这是 python 报的错误，怀疑是 python 使用的 gcc 编译器版本不对导致（因为 gcc 已经升级成了 8.4.0，但是 python 显示的 gcc 编译器还是 4.8.5），在本机用 8.4.0 重新编译一下 python 试试。  
结果：编译 python2.7 解决的，python3 没有成功，原因不详。  

### RuntimeError: gcc \<= 4.8 not supported, please upgrade your gcc
这是因为手动编译的 gcc 安装时软连接没有修改好，运行 `cc --version` 检查一下就会发现输出的版本号依旧是老版本，重新建立下软链接即可。  
`ln -sf /usr/local/bin/gcc /usr/bin/cc` 参考自：[升级的GCC，仍然出现错误：gcc \<=4.8不支持，请升级您的gcc](https://mediasoup.discourse.group/t/upgraded-gcc-still-getting-error-gcc-4-8-not-supported-please-upgrade-your-gcc/76)  

### server 代码 npm install 超时，导致 install 失败
```
cd node_modules
rm -rf clang-tools-prebuilt
rm -rf mediasoup
npm install
```

# 扩展
## 后台启动程序
程序是前台运行的，只要一关闭xshell程序就关掉了。

### pm2
#### 后台启动服务
```cmd
pm2 start server.js
```

#### 后台启动应用
```cmd
pm2 start npm -- start
```
详细介绍请[参考这里](https://blog.csdn.net/pintu274111451/article/details/81843623)

## 参考资料
[【流媒体服务器Mediasoup】环境部署与demo搭建(二)](https://blog.csdn.net/gjy_it/article/details/104423353)  
