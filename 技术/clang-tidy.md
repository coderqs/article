# Clang-Tidy
## 简介
clang-tidy 是一个基于 clang 的 C++ “linter” 工具。其目的是提供一个可扩展的框架，用于诊断和修复典型的编程错误，例如样式违规、接口滥用或可以通过静态分析推断出的错误。clang-tidy 是模块化的，并提供了一个方便的界面来编写新的检查。  

## 安装
### Ubuntu
``` shell
sudo apt-get install clang-tidy-5.0
``` 
### Centos
``` shell
sudo yum install -y centos-release-scl
sudo yum install -y llvm-toolset-7
sudo yum install -y llvm-toolset-7-clang-analyzer llvm-toolset-7-clang-tools-extra
```
#### 启动
``` shell
scl enable llvm-toolset-7 'clang -v'
scl enable llvm-toolset-7 'lldb -v'
scl enable llvm-toolset-7 bash
``` 
## 使用
``` shell
clang-tidy -list-checks -checks='google' test.cpp --
``` 
- `-checks='google'`: 表示检测是否违反google code style;  
- `test.cpp`: 被检测的文件;  
- `--`: 表示这个文件不在compilation database里面，可以直接单独编译;  

## 参考资料
[clang-tidy](https://clang.llvm.org/extra/clang-tidy/)  
[深入研究Clang(十三)clang-tidy简介](https://zhuanlan.zhihu.com/p/102248131)  
[clang-tidy使用总结](https://blog.csdn.net/ypshowm/article/details/100040729)  
[深入研究Clang(十四)clang-tidy的使用](https://blog.csdn.net/snsn1984/article/details/104220921)  
