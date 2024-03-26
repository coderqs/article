---
title: conan 快速使用
description: ""
author: 清松
date: 2024-03-14T14:18:49+08:00
categories:
  - 工具
tags:
  - 制品库
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACY4Y66J6J0PHRWQX90GB
---

## conan 包名约定
标准的conan 包名引用(reference)格式是：`package_name/version@user/channel`，用于在制品仓库中唯一的识别一个包 
- **package_name** 包名
- **version** 版本号
- **user** \[**可选**\]上传包的用户/组织名
- **channel** \[**可选**\]一般用来区分制品的成熟度,比如 `stable`(稳定版本),`testing`(测试版本)
在向制品仓库上传包时，包名中package_name/version是必须要有的字段.user,channel都是可选字段，上传用户在上传包时可以不指定。
### 参考资料
[conan入门(四):conan 引用第三方库示例](https://blog.csdn.net/10km/article/details/122988626)  



## 安装
conan 需要 python3 环境，所以先安装 python3 以及 pip。具备 python3 环境后使用下面的命令安装 conan 
```
pip install conan
```
等待完成即可，完成后使用下面命令来确认是否安装成功
```
conan --version
```

## 基础命令
### 查看远程仓库
```
conan remote list
```

### 添加一个远程仓库
```
conan remote add <repo_name> <repo_url>
```

### 删除远程仓库
```
conan remote remove <repo_name>
```

### 设置仓库的登录用户和密码
```
conan user -p <password> -r <repo_name> <username>
```

### 搜索包
1. 搜索本地仓库
```
conan search <package_name>
```
2. 搜索远程仓库
```
conan search <package_name> --remote <repo_name> 
```

### 查看指定包的信息
```
conan inspect <package_name> 
```

### 创建 conan 包
1. 创建可执行程序的包
```
conan new <project_name>/<version_num>
```
2. 基于模板创建使用 cmake 生成库的 conan 包
```
conan new <project_name>/<version_num> --template cmake_lib
```

### 生成 conan 包
```
conan create <conanfile_path>
```

### 上传到 conan 仓库
```
conan upload <project_name>/<version_num> -r <repo_name> --all
```



## 参考资料
[conan使用(一)--安装和应用](https://www.cnblogs.com/xl2432/p/11873394.html)
[conan入门(一):conan 及 JFrog Artifactory 安装](https://blog.csdn.net/10km/article/details/122987204)  
[conan入门(四):conan 引用第三方库示例](https://blog.csdn.net/10km/article/details/122988626)  
