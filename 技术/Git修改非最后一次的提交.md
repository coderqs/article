---
title: "[Git] 修改非最后一次的提交"
description: 
author: 清松
date: 2024-03-27T10:46:30+08:00
categories:
  - 技术
tags:
  - Git
enableMath: true
url: 
draft: false
series: Git及衍生品
---
## 背景
有时候提交会有一些遗漏，尤其是在添加新文件的时候，如果及时发现还好，可以通过 `--amend` 来解决，但也会有时候发现没那么及时，这时已经又有其他的提交存在了，这时该如何解决？本文记录了一种解决方法。

## 步骤
1. 首先先要确定需要修改的提交的 commit_hash。
2. 使用 `git rebase -i` 命令进入到交互式重写(commit)模式。我是使用的 `git rebase -i HEAD~3`，表示修改最近的 3 个提交（这个数字是可变的，根据自己的需求修改）。
3. 2 中的操作会打开一个编辑窗口（跟 `--amend` 时弹出的窗口一样），展示了最近 3 个提交的列表，如下所示。
```
pick 81d76c7 Commit Message 3
pick 01862cc Commit Message 2
pick d6483ce Commit Message 1
   
# Rebase f82a5cc..d6483ce onto f82a5cc (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
#         create a merge commit using the original merge commit's
#         message (or the oneline, if no original merge commit was
#         specified); use -c <commit> to reword the commit message
# u, update-ref <ref> = track a placeholder for the <ref> to be updated
#                       to this position in the new commits. The <ref> is
#                       updated at the end of the rebase
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
```
4. 确认自己想要修改的提交，将其前面的 `pick` 修改成 `edit` 后保存关掉。例如，我修改的是倒数第二次的提交。
```
pick 81d76c7 Commit Message 3
edit 01862cc Commit Message 2
pick d6483ce Commit Message 1
```
5. 这时就可以进行修改，**但要记住这一次修改完后记得执行 `add` 操作** ，然后再执行 `git commit --amend` 。
6. 执行 `git rebase –continue`。

### 注意点
上述方法在执行后**会修改掉 commit_hash** ，因此这将会影响到其他在此次及之后的提交的基础之上的分支、克隆、分叉等。因此需要**谨慎使用该方法，尤其是主线主线分支上**，如需使用还请与团队成员协商好使用方法。

**补充：我目前使用的时候都是修改的与后续提交没有相同文件的情况，不确定如果和后续提交有相同文件的情况会造成什么影响。**


## 参考资料
[git如何在之前的commit进行修改@不及物动词](https://worktile.com/kb/ask/223193.html)