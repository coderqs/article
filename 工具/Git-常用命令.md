---
title: "[Git] 常用命令整理"
description: 整理一些 Git 的常用场景的命令，方便速查
author: 清松
date: 2023-12-10T23:42:09+08:00
categories:
  - 工具
tags:
  - Git
math: true
url: 
draft: false
series: Git及衍生品
---
## 本地
### 基础命令
#### 创建存储库
``` shell
git init <repositories_name>
``` 
#### 状态查看
``` shell
git status
``` 
#### 文件操作
``` shell
# 添加文件
git add <filename>

# 删除文件
git rm <filename>

# 移动或重命名
git mv <filename_src> <filename_dst>
```
#### 添加更新的文件
```
git add -u
```
#### 提交文件
``` shell
git commit -m "提交内容的描述"
``` 
#### 提交所有追踪的文件
```
git commit -am ""
```

此命令前可以不执行 `add` 直接将发生变更的已跟踪的文件提交
#### 比较差异
``` shell
git diff filename
```
#### 查看提交记录
``` shell
git log

# 简版记录
git log --pretty=oneline
```
#### 查看执行命令的记录
``` shell
git reflog
```
### 撤销
#### 撤销提交(commit)
```
git reset --soft HEAD^
```
**注意**: 该命令只撤销 commit 操作，add 操作还在
**参考自:**《[git commit之后，想撤销commit](https://www.cnblogs.com/lfxiao/p/9378763.html)》
#### 撤销暂存(add)
```
git reset --mixed
```
撤销 add 操作，但文件的修改还在
#### 撤销修改
```
git checkout <filename>
```
#### 撤销跟踪
##### 撤销跟踪，同时递归删除文件
```
 git rm -rf 
```
##### 撤销跟踪，但不删除文件
```
git rm -r --cached <filename>
```
##### 恢复撤销的跟踪
```
git restore --staged <filename>
```

### 追加提交
将这次的提交追加到上次提交中，只产生一次提交记录，一般是用于上次提交有遗漏的文件追加用的，也会用于修改提交记录的说明。有远程仓库的时候，有可能会同时使用到强制推送。
#### 未推送
```
git commit --amend
```
执行命令后会/进入提交信息编辑界面，修改保存退出即可。
#### 已推送

前面与已推送布置相同，push 的时候会要求 pull 合并再推送，或者选择强制推送（**慎用**）


### 版本回退
#### 回退到上个版本
``` 
git reset --hard HEAD^
``` 
#### 回退到上上个版本
``` 
git reset --hard HEAD^^
``` 
#### 回退到上100个版本
``` 
git reset --hard HEAD~100
``` 
#### 回退到指定版本
``` 
git reset --hard <commit_id>
``` 
#### 回退单个文件
```
git reset <commit_id> <filename>
git commit -m "提交描述信息"
git checkout <filename>
```
**参考自:**《[git 如何回退单个文件](https://zhuanlan.zhihu.com/p/87158334)》


### 分支操作
#### 查看分支
```
git branch
```
#### 创建分支
```
git branch <name>
```
#### 切换分支
```
git checkout <name>`或者`git switch <name>
```
#### 创建+切换分支
```
git checkout -b <name>`或者`git switch -c <name>
```
#### 合并某分支到当前分支
```
git merge <name>
```
#### 删除分支
```
git branch -d <name>
```
#### 强制删除分支
```
git branch -D <name>
```
#### 创建一个空的分支
```
git checkout --orphan <nwe_branch_name>
```
这条命令可以创建一个没有提交历史的分支，创建完成后将其余文件删除掉即可 `git rm -rf *`\
**参考自：**《[在GIT中创建一个空分支](https://segmentfault.com/a/1190000004931751)》

## 合并(merge)
### merge覆盖当前分支
#### 方法一：
[git merge覆盖当前分支](https://blog.csdn.net/skysky97/article/details/122847214)
#### 方法二：
```
git checkout <准备被覆盖的分支>
git reset --hard <用来覆盖的分支>
```
**参考自:** [Git 分支合并覆盖主分支问题](https://blog.51cto.com/u_15127619/4349938)

## 远程
### 强制推送（覆盖远端的代码）
```
git push -f <local_branch>:<remote_branch>
```
### pull 强制覆盖本地的代码
```
git fetch --all
git reset --hard origin/<branch_name>
git pull
```
**参考自**: 《[git pull 强制覆盖本地的代码](https://blog.csdn.net/weixin_39289876/article/details/118959936)》
### 删除远端分支
```
git push <remote> --delete <branch>
```
或
```
git push <remote> :<branch>
```
### 同步分支列表
```
git fetch -p
```
**参考自:** [Git 操作——如何删除本地分支和远程分支](https://www.freecodecamp.org/chinese/news/how-to-delete-a-git-branch-both-locally-and-remotely/)

### 删除远程提交记录
[清除Github提交历史记录](/技术/清除Github提交历史记录.md)

### 添加远程仓库并推送
1.  `git remote add origin <remote_repo_add>`
   添加远程库后，其实并没有对远程库做任何事情，远程库还是空的。这里只是添加了本地仓库与远程仓库的关联关系。其中 origin 是远程仓库的一个代名词，可以使用其它任何名称，这里只是惯用法而已。
2.  `git pull origin master --allow-unrelated-histories`
   如果远程仓库有过提交记录，需要将远程仓库的记录和本地合并
3.  `git push origin master`
   添加完关联关系之后，我们就可以将本地仓库推送到远程服务器上，例如我们将本地的 master 分支推送到远程仓库

参考自：[git将代码添加到远程仓库](https://blog.csdn.net/LookOutThe/article/details/122325715)

## 扩展
### git status 显示中文
默认情况下 git status 显示的中文是 utf-8 的编码类似于这样 `\256\276\345\244`
**解决方法：**
```
git config --global core.quotepath false
```
### git修改文件夹名字
```
git mv -f oldfolder newfolder
git add -u newfolder (-u选项会更新已经追踪的文件和文件夹)
git commit -m "changed the foldername whaddup"
```
**参考自:** [git修改文件夹名字](https://www.cnblogs.com/coce/p/9123752.html)  

### 从分支获取所有文件且不合并就放入当前分支
```
git merge <other_branch> --no-commit --no-ff -X theirs
git reset <current_branch>
```
**参考自:** [如何从一个git分支获取所有文件，不合并就放到当前分支？](https://superuser.com/questions/692794/how-can-i-get-all-the-files-from-one-git-branch-and-put-them-into-the-current-b)

### git 查看config配置信息
config 配置有system级别 global（用户级别） 和local（当前仓库）三个 设置先从system-》global-》local  底层配置会覆盖顶层配置 分别使用--system/global/local 可以定位到配置文件
#### 查看系统config
```
git config --system --list
```
#### 查看当前用户（global）配置
```
git config --global  --list
```
#### 查看当前仓库配置信息
```
git config --local  --list
```