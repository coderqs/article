---
title: "[VSCode] 插件 remote_ssh 的使用"
description: 
author: 清松
date: 2023-12-10T22:29:28+08:00
categories:
  - 工具
tags:
  - vscode
  - 远程连接
math: true
url: 
draft: false
series: VisualStudio
---
## 安装
- 在 vscode 的扩展里搜索 `Remote-SSH`  
![扩展中搜索remote-ssh](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E6%89%A9%E5%B1%95%E4%B8%AD%E6%90%9C%E7%B4%A2remote-ssh.PNG)  
- 点击安装，等待安装完成即可，安装完成后会在左侧的侧边栏多出一个图标。  
![安装完成后多出来的侧边栏](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90%E5%90%8E%E5%A4%9A%E5%87%BA%E6%9D%A5%E7%9A%84%E4%BE%A7%E8%BE%B9%E6%A0%8F.PNG)  

## 配置
- 按顺序点击图中 1、2 的图标; 
![新建连接](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E6%96%B0%E5%BB%BA%E8%BF%9E%E6%8E%A5.png)   
- 在图中 3 的位置输出要远程连接的机器的用户名和地址，例如：`ssh root@192.168.13.97`;  
- 输入完成后回车，点击 `C:\User\xxx\.ssh\config` 就可以看到连接已添加进来了，如果没有点击图中 4 的位置刷新;  
![打开config](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E6%89%93%E5%BC%80config.png)  
- 在已添加的连接上右键，选择 `Connect to Host in New Windows`;  
![打开连接](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E6%89%93%E5%BC%80%E8%BF%9E%E6%8E%A5.jpg)  
- 选择目标机器的系统类型（当无法识别目标机器的系统时会弹出该选项）;  
![选择目标机器的系统](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E9%80%89%E6%8B%A9%E7%9B%AE%E6%A0%87%E6%9C%BA%E5%99%A8%E7%9A%84%E7%B3%BB%E7%BB%9F.jpg)  
- 选择 `Connect`;  
- 输入密码;  
![输入密码](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E8%BE%93%E5%85%A5%E5%AF%86%E7%A0%81.png)  
- 输完密码后右下角有提示在下载安装 VS Code Server，等待其完成即可;  
![等待安装完成](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E8%BE%93%E5%85%A5%E5%AF%86%E7%A0%81%E5%90%8E%E7%AD%89%E5%BE%85%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90.jpg)  
- 左下角这里类似这样的显示就是连接完成了;  
![连接成功](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/IDE/vscode/%E8%BF%9E%E6%8E%A5%E6%88%90%E5%8A%9F.png)  

## 参考资料
[使用 SSH 进行远程开发](https://code.visualstudio.com/docs/remote/ssh#_getting-started)  
[玩转VSCode插件之Remote-SSH](https://www.cnblogs.com/liyufeia/p/11405779.html)  
[VSCode:Remote-SSH配置实录](https://blog.csdn.net/sixdaycoder/article/details/89947893)  
