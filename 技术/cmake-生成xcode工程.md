---
title: "使用 cmake 生成 XCode 工程"
description: ""
date: '2019-03-11'
draft: false
authors:
  - "清松"
tags:
  - CMake
  - XCode
  - MAC/IOS
categories:
  - 技术
series:
---

# 使用 cmake 生成 XCode 工程
## 方法
cmake 的构建命令增加参数 \`-G "Xcode"\`，例如  
``` shell
cmake -G Xcode ..
``` 

### 遇到的问题
#### 报错找不到编译器
##### 错误信息
``` text
-- The C compiler identification is unknown

-- The CXX compiler identification is unknown

CMake Error at CMakeLists.txt:2 (project):

 No CMAKE_C_COMPILER could be found.

CMake Error at CMakeLists.txt:2 (project):

 No CMAKE_CXX_COMPILER could be found.
``` 

##### 解决方法
执行命令下面的命令后再重试即可
``` shell
sudo xcode-select --switch /Applications/Xcode.app/
```
**注：**执行完命令后要清空 cmake 生成的缓存文件。

## 参考资料
[Mac cmake生成xcode项目工程](https://blog.csdn.net/song_esther/article/details/105419945)  
