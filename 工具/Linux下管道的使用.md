---
title: Linux下管道的使用
description: ""
author: 清松
date: 2022-10-26T14:11:18+08:00
categories:
  - 工具
tags:
  - 命令
  - Linux
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACY0RQN4989C2RJQDW4G7
---
# 管道的使用
管道在 Linux 命令中使用频率很高，常见的用法是与 `grep` 命令结合使用，例如 `ls -l | grep xxx`、`netstat -nlp | grep 443` 等。但是与其他命令结合使用的时候比较少，而且也不一定能得到想要的结果，例如 `find . -name ".txt" | rm`。这是因为管道是将第一个命令的输出发送到第二个命令的标准输入，而 `rm` 不接受标准输入，因此无法的得到想要的结果，需要结合 `xargs`
来达到想要的结果。

## 用法示例
### 删除 find 找到的文件
```
find . -name ".txt" | xargs rm
find . -name ".txt" | grep "foo" | xargs rm  
```
请注意,如果有任何包含换行符或空格的文件名,则此操作将无法正常工作.要处理包含换行符或空格的文件名,应该使用:
```
find . -name ".txt" -print0 | xargs -0 rm
```
这将告诉`find`使用空字符而不是换行符终止结果.但是,`grep`不会像以前那样工作.而是使用这个:
```
find . -name ".txt" | grep "foo" | tr "\n" "\0" | xargs -0 rm
```
此时间`tr`用于将所有换行符转换为空字符.

### 移动 find 到的文件
```
find . -name ".txt" | xargs -I {} mv {} dest_dir
```

## 参考资料
[Linux为什么我不能管道找到rm的结果？](https://qa.1r1g.com/sf/ask/1421510961/)\
[Linux xargs 命令](https://www.runoob.com/linux/linux-comm-xargs.html)
