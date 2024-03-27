---
title: Github配置免密推送
description: 
author: 清松
date: 2022-03-14T15:29:15+08:00
categories:
  - 应用
tags:
  - Git
  - Github
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXTJKS6GWS9XGVT5J9ZD
---
## github https 免密 push
### 用git证书
```
git config credential.helper store				
```
或
```
git config --global credential.helper store
```
#### 参考资料
[github https免密push](https://blog.csdn.net/u010563350/article/details/106965461?utm_medium=distribute.pc_relevant.none-task-blog-searchFromBaidu-2.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-searchFromBaidu-2.not_use_machine_learn_pai)    
[git配置免密登录](https://liqing.blog.csdn.net/article/details/79065095?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai)    

### ssh 免密
[github本地git push ssh方式免用户名和密码配置相关问题](https://blog.csdn.net/lonyw/article/details/75392410?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai)    

## 将存储库和 ssh key 放在 U 盘里
### 生成新的 SSH 密钥
```
# ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
### 添加 key
```
# ssh-agent bash
# ssh-add $ssh_key_path
```
### 检查保存的
```
# ssh-add -l
```
### 修改ssh配置
```
# vim ~/.ssh/config

#activehacker account
Host github.com-activehacker
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_activehacker

#jexchan account
Host github.com-jexchan
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_jexchan
```
- Host：Git识别名，是一个别名，如果使用 Github 上传下载代码，正常情况下是 `github.com`，如果是多个 Github 账号，则需要起一个别名，建议命名规则为项目名/账户名 .git 服务器，比如`adoredee.github.com`第二个 Host，第一个 Host 为正常命名：`github.com`；
- HostName：服务器地址，Github 地址为`github.com`、GitLab 地址为`gitlab.com`、Gitee 地址为`gitee.com`；
- IdentityFile： 公钥文件所在的绝对路径；

#### 参考资料
[不同github帐户的多个SSH密钥设置](https://gist.github.com/jexchan/2351996)    
[多个SSH密钥并存且连接到Github](https://kangzhiheng.top/post/11-more-ssh-in-one-laptop/)    
[Could not open a connection to your authentication agent](https://blog.csdn.net/argleary/article/details/100638560)   

## 遇到的问题
### github 已配置了 ssl 但每次 push 仍需要输入用户名/密码
因为仓库使用的地址是 https 类型而不是 ssh 类型，修改一下即可。
```
git remote remove origin
git remote add origin git@github.com:<Username>/<Your_Repo_Name>.git
```
完成后还需要重新设置 track branch (注意分支名称)
```
git branch --set-upstream-to=origin/master master
```