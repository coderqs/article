---
title: 在 Linux 系统服务中调用 Shell 脚本报错
description: 记录了配置 service 时调用 shell 脚本，启动服务却提示失败的问题和解决方法。
author: 清松
date: 2023-12-07T23:39:15+08:00
categories:
  - 技术
  - 工具
  - 应用
  - 方法
  - 杂项
  - 作品
tags:
  - Shell
  - Linux
  - 故障解决
math: true
url: 
draft: false
series:
---
## 背景
想要给树莓派添加个服务，每次开机后把 ip 发送到指定邮箱。结果配置好了后总是启动不起来，查看日志发现没有权限 `Failed to execute command: Permission denied`  

``` text
-- Unit send_ip.service has begun starting up.
Sep 14 16:33:01 bogon systemd[11178]: send_ip.service: Failed to execute command: Permission denied
Sep 14 16:33:01 bogon systemd[11178]: send_ip.service: Failed at step EXEC spawning /usr/bin/script/send_ip.sh: Permission denied
-- Subject: Process /usr/bin/script/send_ip.sh could not be executed
-- Defined-By: systemd
-- Support: https://access.redhat.com/support
-- 
-- The process /usr/bin/script/send_ip.sh could not be executed and failed.
-- 
-- The error number returned by this process is 13.
Sep 14 16:33:01 bogon systemd[1]: send_ip.service: Control process exited, code=exited status=203
Sep 14 16:33:01 bogon systemd[1]: send_ip.service: Failed with result 'exit-code'.
Sep 14 16:33:01 bogon systemd[1]: Failed to start 开机后向管理员发送本机的ip.
-- Subject: Unit send_ip.service has failed
-- Defined-By: systemd
-- Support: https://access.redhat.com/support
-- 
-- Unit send_ip.service has failed.
```
已确认脚本确实是正确的，service 文件如下  
``` service
[Unit]
Description=开机后向管理员发送本机的ip
After=network.target

[Service]
Type=forking
User=root
ExecStart=/usr/bin/script/send_ip.sh
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
查了下 `code=exited status=203` 表示 [systemctl 执行脚本时需要知道脚本的解释器](https://blog.csdn.net/shangyexin/article/details/80968202)，可脚本前面明明有写 `#!/bin/bash`

## 原因
203 的错误提示没有错，确实是系统不知道脚本的解释，只是这里不应该是在脚本开头加 `#!/bin/bash`，而是在 `ExecStart` 里声明使用的解释器，将 `ExecStart=/usr/bin/script/send_ip.sh` 修改为 `ExecStart=/bin/bash /usr/bin/script/send_ip.sh`  
修改后的 service 文件应该如下  
``` service
[Unit]
Description=开机后向管理员发送本机的ip
After=network.target

[Service]
Type=forking
User=root
ExecStart=/bin/bash /usr/bin/script/send_ip.sh
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

## 参考资料

[修复systemd服务203 EXEC故障（无此类文件或目录）](https://stackoverflow.com/questions/45776003/fixing-a-systemd-service-203-exec-failure-no-such-file-or-directory)  
