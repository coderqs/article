---
title: Git 子项目（Subtree）命令的使用
description: ""
author: 清松
date: 2023-02-02T16:27:07+08:00
categories:
  - 工具
tags:
  - Git
math: true
url: 
draft: false
series:
---
`subtree` 可以实现一个仓库作为其他仓库的子仓库，对于主项目来说，另一个项目只作为主项目的一个子目录而存在。
与 `subtree` 类似的还有 `submodule`，但不同的是 `submodule` 依旧是已一个仓库的身份存在，如果需要更新，在拉取过后还需要在主项目上提交一次。而 `subtree` 只需要拉取更新即可。
## 使用
### 添加子项目
```
git subtree add --prefix=<cloned_name> <sub_item_uri> <branch> --squash
```
*   `--prefix=<cloned_name>` `<cloned_name>` 是子项目克隆到本地后的目录名字，也可以是多级的，例如：`3rd/xxx`
*   `<sub_item_uri> <branch>` 子项目的地址和分支名
*   `--squash` 该参数表示不拉取历史信息，而只生成一条 commit 信息
### 推送到子项目仓库
```
git subtree push --prefix=<cloned_name> <sub_item_uri> <branch>
```
### 子项目仓库更新
```
git subtree pull --prefix=<cloned_name> <sub_item_uri> <branch> --squash
```
### 移除子项目/切换子项目分支
在添加 `subtree` 的时候是指定了分支的，如果要切换分则支直接移除 `subtree`，再重新加入子项目的分支
```
git rm <subtree_path>
git commit -m ""
git subtree add --prefix=<cloned_name> <sub_item_uri> <branch> --squash
```
### 补充
#### 推送时遍历全部 commit 的问题
使用 split
```
git subtree split --rejoin --prefix=<cloned_name> --branch <branch>
git push <sub_item_uri> <sub_item_uri>:<branch>
```
## 参考资料
1.  [Git 进阶 - 子仓库 subtree](https://www.jianshu.com/p/e9f6ff4e09dc)
2.  [如何在Github上使用Git subtree来管理父子项目](https://segmentfault.com/a/1190000009695399)

