---
title: 给PDF快速添加目录的方法
description: 使用PdgCntEditor快速给PDF添加目录的教程，以及获取目录文本的方法
author: 清松
date: 2024-03-21T10:36:53+08:00
categories:
  - 工具
tags:
  - 技巧
enableMath: true
url: 
draft: false
series: 
slug: 01HSXACXV5CY61Y0WP1HQD9Z12
---
## 背景
因为下载到的一些扫描版的 PDF 数据自己本身没有带目录书签，看起来不太方便，而且页数又多，自己手动添加即麻烦又太浪费时间。在寻找解决方法时找到了 PdgCntEditor，一个可以快速添加目录的软件 ，所以在这里记录下软件的使用和完整的流程。

## 软件介绍
PdgCntEditor 是一款好用的图形化目录文件编辑器，它能够支持PDF 、DjVu 和PDG （也包括bookcontents.dat和catalog.dat）三种格式，您可以使用文本和树形两种迥然不同的编辑模式，也可以编解码PDG目录让你直接研究他的内部数据格式。不过如果您想通过PdgCntEditor 处理 PDF加密文件的话就无能为力了。作者是[老马](https://www.cnblogs.com/stronghorse)

## 主要流程
1. 找到文本版的目录，在 word 或者 notepad++ 修改下目录中多余的符号或者错误/缺失的信息。
2. 打开 PdgCntEditor，再使用 PdgCntEditor 打开要添加目录的 pdf。
3. 将目录信息复制到 PdgCntEditor 中(这里提醒下，目录的换行符得是windows下的 \\r\\n)，切分页码，调整层级、对齐基准页。
4. 保存

![[快速添加PDF目录的方法-打开pdf.png]]
![[快速添加PDF目录的方法-将目录复制进来.png]]
![[快速添加PDF目录的方法-调整目录层级.png]]
![[快速添加PDF目录的方法-pdf选项.png]]

## 获取目录的方法
### 直接获取
1. 京东或者当当等购物网站在商品介绍页里会有目录信息。
2. 去书籍的出版社网站搜索对应的书籍信息，大部分是会有目录信息的。
3. 在豆瓣读书上搜索对应的书籍，也可能会有目录信息。

### 文字识别
当没有办法直接获取到目录信息，那就只能通过文字识别软件将 DPF 的目录识别出来再手动编辑成文本信息。
1. QQ OCR 文字识别（识别出来的格式特别乱，可以将识别的结果交给 chatgpt 整理），请参考[【记录】PDF｜中英文PDF扫描版目录提取（一、QQ+GPT）](https://blog.csdn.net/qq_46106285/article/details/134272245)
2. Abbyy：是专业识别软件，特别大，而且破解版都有风险，正版激活也麻烦，毕竟我们只是识别个目录不需要识别全部，不是很推荐。
3. Acrobat：它的文字识别会把文字做成一个一个编辑框，识别完成之后并不方便一键复制，更关键的是它如果激活得不到位，就无法识别中文，只能识别英文和数字。
4. PDFElement：又称万兴PDF编辑器，就是对很多人来说安装门槛/价格比较高。

### 其他
如果上面两种方法都搞不定的话，建议就不要弄了，忍忍看完就得了。
  
## 参考资料
[PDF目录编辑软件-PdgCntEditor](https://www.jianshu.com/p/498b0f8bb650)
[PdgCntEditor系列教程一：基础知识](https://www.cnblogs.com/stronghorse/p/11519730.html)
[【工具】FreePic2PDF+PdgCntEditor｜PDF批量添加书签（Windows）]()
[PDF目录编辑软件-PdgCntEditor](https://www.jianshu.com/p/498b0f8bb650)