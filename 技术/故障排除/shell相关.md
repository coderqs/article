---
title: "Shell 脚本相关的问题的解决方法"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - shell
  - 故障排除
categories:
  - 技术
series:
---

# Shell 脚本相关的问题的解决方法
## "-bash: !": event not found
#### 原因
`!` 是 bash 的特殊字符，用于引用之前以!后面字符串开头的最后一个的命令。  
`!$` 是获取上一条命令的最后一个参数。  
**注意**：在脚本中，所有历史命令都被禁用，因为它们只在交互式 shell 中才有意义  

#### 解决方法
##### 方法一
执行 `set +H` 临时关闭该功能

##### 方法二
将该字符串用单引号括起来，双引号不可以

##### 方法三
在 `!` 前添加`\`或后面添加空格，**但这种方法有额外的字符混进去**，所以如果是输入密码的时候出现的问题，这种方法是不可用的。  
  
参考自[What is “-bash: !”: event notfound"](https://serverfault.com/questions/208265/what-is-bash-event-not-found)
