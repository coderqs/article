---
title: Linux 下查看库的符号
description: 查看动态库中的符号
author: 清松
date: 2022-01-22T14:05:37+08:00
categories:
  - 技术
tags:
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXNRP0PQ55BTG41GAEZM
---
## 查看生成的 so 是否存在符号未定义的内容
``` shell
ldd -r xxx.so
```
## 查看 so 中的所有符号
``` shell
nm xxx.so
```

例如下面是 nm 输出的结果中的一段:  
```
                 U _ZN11CHttpParser20GetCurrentHttpMethodER13http_method_t
                 U _ZN11CHttpParser26ExactResultFromHttpMsgBodyESsRSs
                 U _ZN11CHttpParser5parseESs
                 U _ZN11CHttpParserC1ESs
00000000000134e6 W _ZN5boost10shared_ptrI11CHttpParserE4swapERS2_
0000000000012e56 W _ZN5boost10shared_ptrI11CHttpParserEC1ERKS2_
0000000000012e30 W _ZN5boost10shared_ptrI11CHttpParserEC1Ev
0000000000014cda W _ZN5boost10shared_ptrI11CHttpParserEC1IS1_EEPT_
0000000000012cda W _ZN5boost10shared_ptrI11CHttpParserED1Ev
0000000000013df4 W _ZN5boost10shared_ptrI11CHttpParserEaSERKS2_
000000000001348b W _ZN5boost14checked_deleteI11CHttpParserEEvPT_
```
其输出的结果是以 `地址 类型 符号名` 的结构显示的，其中一些常见的符号类型如下  

| nm 输出字符 | 含义                                                                                       |
| :-----: | :--------------------------------------------------------------------------------------- |
|    R    | Read only symbol. 比如在代码中有一个const MAXDATA = 3095; 则MAXDATA就是一个Read only symbol            |
|    N    | 这是一个调试符号                                                                                 |
|    D    | 这是一个已经初始化的变量的符号。比如代码中int i = 1和char \*str = "Hello"则i和str都是这种类型的符号                       |
|    T    | Text段的符号。子程序都是这种符号，比如文件中实现了一个函数function，则function就是这种符号                                  |
|    U    | 未定义的符号。如果文件中引用了不存在的函数，则这些未定义的函数符号就是这种类型                                                  |
|    S    | 未初始化的符号，比如全局变量int s;则s的符号就是此类型                                                           |

## 参考资料
[Linux下使用nm命令排查和解决“undefined referenceto”](https://blog.csdn.net/acs713/article/details/13505931)  
[undefined symbol问题的查找、定位与解决方法](https://blog.csdn.net/buknow/article/details/96130049)