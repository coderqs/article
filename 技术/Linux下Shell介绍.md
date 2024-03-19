---
title: Linux 下的 Shell 介绍
description: ""
author: 清松
date: 2024-03-13T15:04:26+08:00
categories:
  - 工具
tags:
  - Shell
enableMath: true
url: 
draft: false
series:
---
## 基础语法

### 注释
#### 单行注释
使用 `#`  
``` shell
# echo "hello"
```
#### 多行注释
##### 方法一
``` shell
    : << !
    这是注释1
    这是注释2
    !
```

##### 方法二
``` shell
    :'
    这是注释1
    这是注释2
    '
```
##### 方法三
``` shell
    : << 字符   #这里的字符可以是数字或者是字符都可以
    这是注释1
    这是注释2
    字符        #这里的字符要和一开始的一样
```
##### 方法四
``` shell
    if false; then
    这是注释1
    这是注释2
    fi
```
##### 方法五
``` shell
    ((0))&&{
    这是注释1
    这是注释2
    }
```
#### 参考资料
[shell中的单行注释和多行注释](https://blog.csdn.net/lansesl2008/article/details/20558369/)  

