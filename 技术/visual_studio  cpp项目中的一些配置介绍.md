---
title: "Visual Studio 的 C++ 项目中的一些配置的介绍"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - Visual Studio
categories:
  - 技术
series:
---

# Visual Studio 的 C++ 项目中的一些配置的介绍
## VC++包含目录和 C++ 附加包含目录的区别
c/c++ 附加包含目录，代表的是c/c++文件编译时所需要的头文件，而资源编译时也是需要附加包含库目录的，而
vc++ 的包含目录，代表的是全局项目的包含目录。配置过VC++里面的库，C/C++里面的就可以不用配置。  

### 参考资料
[VC包含目录和c/c++附加包含目录的区别](https://blog.csdn.net/qq_35608277/article/details/80768660)  

## 默认的属性文件
### vs2017
1. Microsoft.Cpp.x64.user 系统默认的属性表，全路径为 `C:\Users\horswing\AppData\Local\Microsoft\MSBuild\v4.0\Microsoft.Cpp.x64.user.props` 项目创建后，默认有这个属性表。双击可以修改（效果与 `solution explorer 项目名上右键 -> property` 一致），右键选则 remove 和移除。
2. Application 表示这个项目生成的是一个“应用程序”（不是DLL或LIB）。在 Property Manger 里，这项是不能改的，所以你发现双击后，出现的页面是灰色的，右键也只有 property 选项。在哪里改呢？在 solution explorer 里的 `项目属性 -> gerneral -> Project Defaults -> Configuration Type`.  
3. Unicode Support 和 Core Windows Libraries 和 Application 项一样，这两项也是“只能看不能改的”，要改分别在 `项目属性 -> gerneral -> Project Defaults` 里的 `Character Set` 和 `Use of MFC` 修改。  

### vs2019
1.  vs2019 相较于 vs2017 少了 Microsoft.Cpp.x64.user 多了 whole program optimization

### 参考资料
[visual studio属性管理器（property manager）上各项的含义](https://blog.csdn.net/wu_nan_nan/article/details/70054845)

## 多张属性表叠加
多张属性表一起使用时，两张表定义了相同的属性，后面的表的配置优先。  
例如，当两张表同时定义了 **"附加包含目录"**，在没有选择继承的情况下，则只有后面导入的属性表的该项配置生效，如果选择了继承则同时生效（因为后一张属性表将前一张属性表的该项配置继承了，本质上还是后一张属性表的配置生效）  
> Property sheets are a nice way to set up properties to projects. Each
property sheet is a collection of properties for a project. One can
attach arbitrarily many property sheets to each project, and the
property sheets can be shared between projects. The latter feature is
the essential one. 
>
> In my solutions at least, the projects share very similar properties.
> Now I can create just one property sheet for the whole solution and
> apply that to each project. If I want to change the properties, I will
> do so in the property sheet.
>
> What’s more, multiple property sheets can be layered so that the union
> of their properties applies. The sheets are given an order. If two
> sheets define the same property, then the later one takes priority.
>
> The strategy is to give each solution a property sheet in which to
> configure output directories, disable warnings, and disable secure-stl
> etc. You will then make this property sheet part of your solution, in
> the sense of carrying it around in the version control.
>
> There are also special global property sheets called
> Microsoft.Cpp.Win32.user and Microsoft.Cpp.Win64.user which are
> automatically added to each configuration of each project. These will
> apply properties globally on your computer. If you change to another
> computer, these settings are lost. These property sheets are ideal for
> specifying include and library directories for external libraries
> (which are of course computer-specific). While the former works on
> 32-bit builds, the latter works on 64-bit builds. Of course, you will
> want to choose different directories for them.
>
> A bit odd feature of the property sheets is that they won’t get save
> automatically when you change a property. You must either Save All, or
> right click on the property sheet and save it. This is unintuitive and
> causes unnecessary confusion from time to time.
>
> It is useful to notice that a property sheet can be added to all
> projects and configuration at the same time. Simply select the desired
> projects or configurations and right click to add an existing property
> sheet. Unfortunately, it seems a given property sheet can not be
> removed from all projects at once.
> 
> If you need to set project-specific properties, do note that you must
> explicitly bring in the inherited properties. For example, in the
> Preprocessor definitions property, this is done by
> %(PreprocessorDefinitions).
> 
> There is a trap in the command-line settings. If you specify
> Additional Options in a project, then those will not be unioned with
> the additional options in the property sheets. Unless I am mistaken,
> it is missing a way to bring in the inherited options. Therefore, you
> should use the other options explicitly instead. For example, if you
> need a preprocessor definition, do it in the Preprocessor definitions
> property instead of as a /D switch in Additional Options.

### 参考资料
[Property sheets in Visual Studio 2010](https://kaba.hilvi.org/homepage/blog/shorties-2012.htm)  
