---
title: Python 的编译安装
description: 
author: 清松
date: 2024-01-22T14:39:31+08:00
categories:
  - 工具
tags:
  - python
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXZ4E9K3M9SMQ419NJM4
---
## 准备
访问[官网](https://www.python.org/downloads/source/)找到对应版本（以Python 3.6.5为例）如图：  
![python_安装文件](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%AF%AD%E8%A8%80/%E7%A8%8B%E5%BA%8F%E8%AF%AD%E8%A8%80/python/python_%E5%AE%89%E8%A3%85%E6%96%87%E4%BB%B6.png)  
下载命令：
```
wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz
```

## 安装
### 解压
```
tar -zxvf Python-3.6.5.tgz
```

### 准备编译环境
```
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```

如果python是3.7版本，还需要安装 `libffi-devel`  

### 编译
```
cd Python-3.6.5
./configure --prefix=/usr/bin/python-3.6.5 && make && make install
```

其中 `--prefix` 是 Python 的安装目录，安装成功后如图所示:  
![python_编译安装成功](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%AF%AD%E8%A8%80/%E7%A8%8B%E5%BA%8F%E8%AF%AD%E8%A8%80/python/python_%E7%BC%96%E8%AF%91%E5%AE%89%E8%A3%85%E6%88%90%E5%8A%9F.png)  
*从图中可以看出也同时安装了 setuptools 和 pip 工具*

### 创建软连接
```
ln -s /usr/bin/python-3.6.5/bin/python3.6 /usr/bin/python3
```

### 配置环境变量
编辑环境变量文件 `~/.bash_profile`  
```
# 配置python
export PYTHON_HOME=/root/training/Python-3.6.5
export PATH=$PYTHON_HOME/bin:$PATH
```
保存并执行 `source ~/.bash_profile`，查看是否生效。  

## 参考资料
[Linux系统安装Python3环境（超详细](https://blog.csdn.net/L_15156024189/article/details/84831045)  
