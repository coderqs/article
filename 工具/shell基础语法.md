---
title: shell 基础语法
description: ""
author: 清松
date: 2023-09-06T18:29:58+08:00
categories:
  - 工具
tags:
  - Shell
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXZ0988SDZZHZYVP0S5M
---
## 变量 
### 动态变量
动态变量是指取变量时变量名从变量中动态得到，而不是直接的字面量变量，语法如下
```
function fun() {
    local var="$1"
    echo "${!var}"
}

fun 1           # 输出结果是 1，fun 中的 echo 等价于 echo "${1}"
fun 2 val2      # 输出结果是 val2，fun 中的 echo 等价于 echo "${2}"
```

**参考自**: [shell ${!var}的意思 ${!}](https://blog.csdn.net/ab411919134/article/details/102469113)  

### 判断变量是否存在
网上常见的做法是 
```
if [ -z "$var" ]; then 
    echo "var is blank"
else 
    echo "var is set to '$var'"
fi
```
但**这种方法是错误**的，因为它不区分未设置的变量和设置为空字符串的变量，当 `var=”“` 时这种做法同样会输出 `var is blank`。**正确的方式是**
```
if [ -z ${var+x} ]; then
    echo "var is unset"
else 
    echo "var is set to '$var'"
fi
```
其中的 `${var+x}` 表示如果 `var` 未设置则返回空，否则则替换成字符串 `x`(**注:** 这里的 `x` 可以换成其他的字符串，但**推荐**用 `x`)，当 `var` 是动态变量时判断条件写为 ` [ -z ${!var+x} ]`  
**参考自**: [如何检查是否在 Bash 中设置了变量](https://stackoverflow.com/questions/3601515/how-to-check-if-a-variable-is-set-in-bash)  

## 路径
### 获取被执行脚本所在的路径
#### 方法 1
```
SHELL_FOLDER=$(cd "$(dirname "$0")";pwd)
```

#### 方法 2
```
script_abs=$(readlink -f "$0")
script_dir=$(dirname $script_abs)
```
该语句在 mac 下执行不正确，linux 下正常

## 脚本
### 脚本传入参数的默认值
```
S_THREAD_NUM=${2:-$(nproc)}
S_DEPLOY_NUM=${3:-1}
S_LOCAL_IP=${4:-`hostname -I`}
```

### 输入参数的个数
```
$#
```

## 逐行读取文件
### 方法1：while循环中执行效率最高，最常用的方法
```
function while_read_LINE_bottm(){
    While read LINE
    do
        echo $LINE
    done  < $FILENAME
}
```

### 方法2：重定向法
```
Function While_read_LINE(){
    cat $FILENAME | while read LINE
    do
        echo $LINE
    done
}
```

### 方法3：文件描述符法
```
Function while_read_line_fd(){
    Exec 3<&0
    Exec 0<$FILENAME
    While read LINE
    Do
    Echo $LINE
    Exec 0<&<3
}
```

### 方法4：for 循环
```
function  for_in_file(){
    For i in  `cat $FILENAME`
    do
        echo $i
    done
}
```

### 总结
for语句效率最高，而在while循环中读写文件式执行效率最高。

### 参考资料
[Shell逐行读取文件的4种方法](https://www.jb51.net/article/59041.htm)

## shell中if条件字符串、数字比对，[[ ]]和[ ]区别
[shell中if条件字符串、数字比对，\[\[ \]\]和\[\]区别](https://blog.csdn.net/weixin_34355881/article/details/94624318?spm=1001.2101.3001.6650.13&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13-94624318-blog-57416906.t0_layer_searchtargeting_s&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13-94624318-blog-57416906.t0_layer_searchtargeting_s&utm_relevant_index=19)

## shell 函数的返回值和处理结果的区别
 - **函数的返回值**是表示函数的退出状态，是通过 `return` 语句实现的，如果中没有则使用最后一条命令的退出状态作为返回值。返回值可以通过`$?`捕获
 - **函数的处理结果**是表示经过函数处理后想要返回的结果。想要得到处理结果一般有两种方法：
    - 借助全局变量，将得到的结果赋值给全局变量；
    - 在函数内部使用 `echo`、`printf` 命令将结果输出，在函数外部使用`$()`或者` `` `捕获结果。

参考自: [Shell函数返回值（return关键字）](http://c.biancheng.net/view/2863.html)

## 遇到的错误
### bad substitution
参考 [Bash 错误替换语法错误：简单快速的修复](https://codefather.tech/blog/bash-error-bad-substitution/)  
