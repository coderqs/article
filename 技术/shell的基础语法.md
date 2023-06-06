---
title: "Shell 基础语法"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - shell
categories:
  - 技术
series:
---

# Shell 基础语法
## 注释
### 单行注释
使用 `#`  
``` shell
# echo "hello"
```

### 多行注释
#### 方法一
``` shell
    : << !
    这是注释1
    这是注释2
    这是注释3
    !
```

#### 方法二
``` shell
    :'
    这是注释1
    这是注释2
    这是注释3
    '
```

#### 方法三
``` shell
    : << 字符   #这里的字符可以是数字或者是字符都可以
    这是注释1
    这是注释2
    这是注释3
    字符        #这里的字符要和一开始的一样
```

#### 方法四
``` shell
    if false; then
    这是注释1
    这是注释2
    这是注释3
    fi
```

#### 方法五
``` shell
    ((0))&&{
    这是注释1
    这是注释2
    这是注释3
    }
```

## 参考资料
[shell中的单行注释和多行注释](https://blog.csdn.net/lansesl2008/article/details/20558369/)  
