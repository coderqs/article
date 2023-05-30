# adb 的使用
## 简介
Android 调试桥 (adb) 是一种功能多样的命令行工具，可让您与设备进行通信。adb 命令可用于执行各种设备操作（例如安装和调试应用），并提供对 Unix shell（可用来在设备上运行各种命令）的访问权限。它是一种客户端-服务器程序，包括以下三个组件：  
- **客户端**：用于发送命令。客户端在开发计算机上运行。您可以通过发出 adb 命令从命令行终端调用客户端。  
- **守护程序(adbd)**：用于在设备上运行命令。守护程序在每个设备上作为后台进程运行。  
- **服务器**：用于管理客户端与守护程序之间的通信。服务器在开发机器上作为后台进程运行。  

adb 包含在 Android SDK 平台工具软件包中。您可以使用 [SDK 管理器](https://developer.android.google.cn/studio/intro/update#sdk-manager)下载此软件包，该管理器会将其安装在 `android_sdk/platform-tools/` 下。或者，如果您需要独立的 Android SDK 平台工具软件包，也可以点[击此处进行下载](https://developer.android.google.cn/studio/releases/platform-tools)。

如需了解如何连接设备以使用 ADB，包括如何使用 Connection Assistant 对常见问题进行排查，请参阅[在硬件设备上运行应用](https://developer.android.google.cn/studio/run/device)。

## 使用
### 常用命令
``` shell
adb connect 10.160.93.94    // 连接
adb disconnect              // 断开连接
adb devices                 // 查看设备列表
adb shell
``` 
### 文件操作
#### 从设备获取文件
``` shell
adb pull /path_in_devices/src_file /local_dst_path
``` 
#### 发送文件到设备
``` shell
adb push /local_src_path/src_file /path_in_devices/dst_path/
``` 

### 安装应用
#### 应用程序安装-将一个包推送到设备上并安装它
``` shell
adb install test.apk
``` 
#### APP安装-将多个APK推送到一个包的设备上并安装它们
``` shell
adb install-multiple test.apk test2.apk
``` 
#### 应用程序安装-将一个或多个包推送到设备上，并以原子方式安装它们
``` shell
adb install-multi-package test.apk demo.apk
``` 
#### 替换现有应用程序，重新安装现有的应用程序，保存其数据
``` shell
adb install -r test.apk
``` 
#### 允许测试包
``` shell
adb install -t test.apk
``` 
#### 允许版本代码降级，仅可调试器包
``` shell
adb install -d test.apk
``` 
#### 授予所有运行时权限，授予应用程序清单中列出的所有权限
``` shell
adb install -g test.apk
``` 
#### 使应用程序作为临时安装应用程序安装
``` shell
adb install --instant test.apk
``` 
#### 使用快速部署
``` shell
adb install --fastdeploy test.apk
``` 
#### 始终按APK到设备和调用包管理器作为单独的步骤
``` shell
adb install --no-streaming test.apk
``` 

## 参考资料
[Android Studio - Android 调试桥(adb)](https://developer.android.google.cn/studio/command-line/adb)  
[adb 发送文件到Android设备和从Android手机复制文件](https://blog.csdn.net/ezconn/article/details/85682916)  
[adb安装apk](https://blog.csdn.net/wqq1027/article/details/105510152)  
