---
title: VS-CPP项目错误记录
description: VisualStudio 下的 C++项目中遇到的一些错误的记录及部分解决方法
author: 清松
date: 2019-01-22T15:56:47+08:00
categories:
  - 技术
tags:
  - C/CPP
  - VisualStudio
math: true
url: 
draft: false
series:
---
记录了一些遇到的错误

## \[error c2275\] 将此类型用作表达式非法
### 错误提示
```
error C2275: 'UNICODE_STRING' : illegal use of this type as an expression
```
### 原因
将C代码在VC++中编译，经常会出现error C2275错误，结果是变量的定义位置不对，应该在函数块的最前面。这是一个编程习惯的问题。
### 解决方法
把变量的声明全部放在变量的生存块的开始
### 参考资料
[error C2275 将此类型用作表达式非法](https://blog.csdn.net/tkp2014/article/details/47048417)


## \[warning C4727\] 具有相同时间戳的名为 xxx.pch 的 PCH 已存在于 aaa.obj 和 bbb.obj 中。

### 错误提示
```
LINK : warning C4727: 具有相同时间戳的名为 xxx.pch 的 PCH 已存在于 aaa.obj 和 bbb.obj 中。  使用第一个 PCH。
```
### 原因
简单点儿说就是重复生成了预编译头了（详细的原因可以看参考资料中的解释）  
### 解决方法
1. 将整个项目的预编译头改为`使用(/Yu)`，(设置的路径为 项目右键 -> 属性 -> C/C++ -> 预编译头)
2. 找到文件 `stdafx.h` 将其预编译头改为`创建(/Yc)`
3. 清理一下重新编译即可。
### 参考资料
[LINK : warning C4727](https://blog.csdn.net/wuan584974722/article/details/80668186)  

## \[error c10100b1\] Failed to load file
### 背景
工作的时候某个项目突然出现的，该项目之前应该也编译过是没问题的。vs是vs2010的
### 解决方法
也不知道这个错误为什么会出现，原因也不太清楚，最后是将属性->链接器->常规->输出文件 从空改成了`$(OutDir)$(ProjectName)$(TargetExt)`。  
按理来说上面的改动和原来默认的输出应该是一样的，但是就是得这么手动输入以下就解决了。

## \[error MSB3073\] 命令postmake_w64.bat
### 错误提示
```
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\MSBuild\Microsoft\VC\v160\Microsoft.CppCommon.targets(153,5): error MSB3073: 命令“postmake_w64.bat "G:\tmp\RelayRC\src\RelayRC\RouterCenter4\\" RouterCenter G:\tmp\RelayRC\bin\w64r_19\  w64r_10
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\MSBuild\Microsoft\VC\v160\Microsoft.CppCommon.targets(153,5): error MSB3073: :VCEnd”已退出，代码为 
```
### 原因
不明，只找到了类似的问题《[错误 1122 error MSB3073: 命令“copy ..\..\3rd\staticlib\openblas\x64\*.dll ..\..\Build\cpu\Debug\x64\ cop](https://blog.csdn.net/ture_dream/article/details/74940083)》，但解决了该问题。
### 解决方法
项目右键打开 “属性”->“生成事件”->“后期生成时间”->“在生成中使用”->选择“否”。

## \[error LNK2038\] 检测到 \_ITERATOR\_DEBUG\_LEVEL 的不匹配项 值 0 不匹配值 2
这个错误出现的时候有些莫名其妙，没的也莫名其妙，虽然没有用到查询到的方法，在此记录一些以备它用
### 错误提示
```
error LNK2038: 检测到“_ITERATOR_DEBUG_LEVEL”的不匹配项: 值“0”不匹配值“2” 
```
### 形成原因  
原因是 Debug 使用了 Release 的库文件。   
即使连接库里面两个都添加着，但是release库文件放在了debug前面，也是出错的。默认按顺序使用库文件。  
### 镜像错误    
```
error LNK2038: 检测到“_ITERATOR_DEBUG_LEVEL”的不匹配项: 值“2”不匹配值“0”.
```
原因是 Release 下使用了 Debug 的库文件
### 参考资料
[检测到“_ITERATOR_DEBUG_LEVEL”的不匹配项: 值“0”](https://blog.csdn.net/caimagic/article/details/51055988)

## \[error LNK2001\] 无法解析的外部符号
在类内添加了内联静态成员函数并实现了之后（之前也有一样的成员函数，但没有报错）编译时报了该错误
```
错误	1	error LNK2001: 无法解析的外部符号 "public: static void __cdecl BaseTestFixtures::NormalParamPublishResource(void)" (?NormalParamPublishResource@BaseTestFixtures@@SAXXZ)	F:\share\butelmeetingsdk_29166\pjsip-apps\libbutelmeeting\ut_case_publish_resource.obj
```
[Error LNK2001 无法解析的外部符号 的几种情况及解决办法](https://blog.csdn.net/shufac/article/details/52041758)

## \[error C1189\] Building MFC application with  MD\[d\]
```
错误	2	error C1189: #error :  Building MFC application with /MD[d] (CRT dll version) requires MFC shared dll version. Please #define _AFXDLL or do not use /MD[d]	D:\VisualStudio\2010\VC\atlmfc\include\afx.h	24
```

## \[error C2385\] 对 成员函数 的访问不明确
### 问题描述
在使用gtest做单元测试的时候使用了`TestWithParam`做参数化编译时发现报错
```
对"SetUpTestCase"的访问不明确
对"TearDownTestCase"的访问不明确
```
### 解决方法
原因是因为形成了钻石继承
```
class A : public testing::Test{}
class B : public A, public testing::TestWithParam<T> {}
```
因为`TestWithParam`是继承自`testing::Test`和`WithParamInterface<T>`所以形成了钻石继承，只要将B改成`WithParamInterface<T>`即可，如下
```
class A : public testing::Test{}
class B : public A, public testing::WithParamInterface<T> {}
```

## \[error c2243\] 类型转换  转换存在，但无法访问
### 问题描述
>错误	3	error C2243: “类型转换”: 从“BMSDKTest_Base_Init_Uninit__CorrcetParam_Test *”到“testing::Test *”的转换存在，但无法访问	E:\project\svn\SipGateWayWorker\butelmeetingsdk_29166\pjsip-apps\libbutelmeeting\inc\googletest\gtest\internal\gtest-internal.h	472
### 解决方法
因为 c++ 默认的是 private 继承，所以显示声明成 public 即可
### 参考资料
[error c2243:"类型转换" 转换存在，但无法访问](https://blog.csdn.net/vsooda/article/details/7874835d)

## \[error C4430\] 缺少类型说明符
### 问题描述
> error C4430: 缺少类型说明符 - 假定为 int。注意: C++ 不支持默认 int	  
> error C2238: 意外的标记位于“;”之前  	

错误代码定位
```
vector<TransportResourceInfo> result;
```
### 解决方法
#### 解决方法 1：
添加头文件  
```
#include <vector>
```
发现依旧报错。
#### 解决方法 2：  
在方法1的基础上，添加名字空间`std::`，代码改为
```
std::vector<TransportResourceInfo> result;
```

## \[error LNK2001\] MSVCRT.lib(crtexew.obj) error LNK2001 无法解析的外部符号
# 问题描述
```
MSVCRTD.lib(crtexew.obj) : error LNK2019: 无法解析的外部符号 _WinMain@16，该符号在函数 ___tmainCRTStartup 中被引用 
Debug\jk.exe : fatal error LNK1120: 1 个无法解析的外部命令

error LNK2001: unresolved external symbol _WinMain@16
debug/main.exe:fatal error LNK 1120:1 unresolved externals
error executing link.exe;
```
# 原因及解决办法
产生这个问题的真正原因是c语言运行时找不到适当的程序入口函数，一般情况下，如果是windows程序，那么WinMain是入口函数，在VS2010中新建项目为“win32项目”
如果是dos控制台程序，那么main是入口函数，在VS2010中新建项目为“win32控制台应用程序” 而如果入口函数指定不当，很显然c语言运行时找不到配合函数，它就会报告错误。修改设置适应你的需求。
### 解决方法
#### 如果你需要的是windows程序：
1. 菜单中选择 Project->Properties, 弹出Property Pages窗口
2. 在左边栏中依次选择：Configuration Properties->C/C++->Preprocessor,然后在右边栏的Preprocessor Definitions对应的项中删除_CONSOLE, 添加_WINDOWS.
3. 在左边栏中依次选择：Configuration Properties->Linker->System,然后在右边栏的SubSystem对应的项改为Windows(/SUBSYSTEM:WINDOWS)
#### 如果你需要的是控制台程序：
1. 菜单中选择 Project->Properties, 弹出Property Pages窗口
2. 在左边栏中依次选择：Configuration Properties->C/C++->Preprocessor,然后在右边栏的Preprocessor Definitions对应的项中删除_WINDOWS, 添加_CONSOLE.
3. 在左边栏中依次选择：Configuration Properties->Linker->System,然后在右边栏的SubSystem对应的项改为CONSOLE(/SUBSYSTEM:CONSOLE)
### 参考资料
[MSVCRT.lib(crtexew.obj) : error LNK2001: 无法解析的外部符号](http://msuyu.lofter.com/post/1d4c6221_bce2f00)
