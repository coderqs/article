---
title: CMake-编译Android平台代码
description: 
author: 清松
date: 2023-08-31T16:12:22+08:00
categories:
  - 工具
tags:
  - 交叉编译
  - Android
  - CMake
enableMath: true
url: 
draft: false
series:
---
## 参考资料
[【Cmake】利用NDK进行Android的交叉编译（附实例）](https://blog.csdn.net/qq_38410730/article/details/103622813)  
[在命令行下用cmake交叉编译可在android中运行的so包](https://blog.csdn.net/MingHuang2017/article/details/78938852)  

## 遇到的问题
### is not able to compile a simple test program.
错误提示
```
CMake Error at /usr/local/share/cmake-3.10/Modules/CMakeTestCCompiler.cmake:52 (message):
  The C compiler

    "/opt/android-ndk/android-ndk-r13b/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi-gcc"

  is not able to compile a simple test program.

  It fails with the following output:

    Change Dir: /home/hantao/MeetingQosInfoReportTools/builds/android/build/CMakeFiles/CMakeTmp
    
    Run Build Command:"/opt/rh/devtoolset-10/root/usr/bin/gmake" "cmTC_a4120/fast"
    /opt/rh/devtoolset-10/root/usr/bin/gmake -f CMakeFiles/cmTC_a4120.dir/build.make CMakeFiles/cmTC_a4120.dir/build
    gmake[1]: 进入目录“/home/hantao/MeetingQosInfoReportTools/builds/android/build/CMakeFiles/CMakeTmp”
    Building C object CMakeFiles/cmTC_a4120.dir/testCCompiler.c.o
    /opt/android-ndk/android-ndk-r13b/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64/bin/arm-linux-androideabi-gcc -target armv7-none-linux-androideabi --sysroot=/opt/android-ndk/android-ndk-r13b/platforms/android-21/arch-arm  -isystem /opt/android-ndk/android-ndk-r13b/platforms/android-21/arch-arm/usr/include  -g -DANDROID -ffunction-sections -funwind-tables -fstack-protector-strong -no-canonical-prefixes -march=armv7-a -mfloat-abi=softfp -mfpu=vfpv3-d16 -fno-integrated-as -mthumb -Wa,--noexecstack -Wformat -Werror=format-security -g -DANDROID -ffunction-sections -funwind-tables -fstack-protector-strong -no-canonical-prefixes -march=armv7-a -mfloat-abi=softfp -mfpu=vfpv3-d16 -fno-integrated-as -mthumb -Wa,--noexecstack -Wformat -Werror=format-security   -O0 -fno-limit-debug-info  -fPIE   -o CMakeFiles/cmTC_a4120.dir/testCCompiler.c.o   -c /home/hantao/MeetingQosInfoReportTools/builds/android/build/CMakeFiles/CMakeTmp/testCCompiler.c
    arm-linux-androideabi-gcc: error: armv7-none-linux-androideabi: No such file or directory
    arm-linux-androideabi-gcc: error: unrecognized command line option '-target'
    arm-linux-androideabi-gcc: error: unrecognized command line option '-fno-integrated-as'
    arm-linux-androideabi-gcc: error: unrecognized command line option '-fno-integrated-as'
    arm-linux-androideabi-gcc: error: unrecognized command line option '-fno-limit-debug-info'
    gmake[1]: *** [CMakeFiles/cmTC_a4120.dir/build.make:66：CMakeFiles/cmTC_a4120.dir/testCCompiler.c.o] 错误 1
    gmake[1]: 离开目录“/home/hantao/MeetingQosInfoReportTools/builds/android/build/CMakeFiles/CMakeTmp”
    gmake: *** [Makefile:126：cmTC_a4120/fast] 错误 2
    

  

  CMake will not be able to correctly generate this project.
Call Stack (most recent call first):
  CMakeLists.txt:3 (project)


-- Configuring incomplete, errors occurred!

```

官方提供的 .cmake 默认使用的是 clang 编译器，我的系统中没有 clang 编译器，需要将 `ANDROID_TOOLCHAIN` 设置为 `gcc` 即可(即在命令行中增加`-DANDROID_TOOLCHAIN="gcc"`)。

**注: 网上有的方法是将 `CMAKE_C_COMPILER_WORKS` 和 `CMAKE_CXX_COMPILER_WORKS` 设置为 1，这样可以绕过 cmake 生成时的报错，但是编译时依旧会编不过，因为这是针对 clang 编译器的解决方法**