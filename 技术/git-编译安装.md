# git 编译安装
## 准备
### 安装依赖
``` shell
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install -y gcc perl-ExtUtils-MakeMaker
```
### 下载源码
``` shell
git clone https://github.com/git/git.git
```
**注：如果没有给 git [配置代理](/工具/编程工具/版本控制/git/配置代理)的话，建议直接下载压缩包会更快些。**  

## 编译
### 编译命令
``` shell
make prefix=/usr/local/git all 
make prefix=/usr/local/git install
```
这里提醒下，如果 git 的源码是从 windows 下拷过来的要注意下文件的可**执行权限**  

### 创建软连接
``` shell
ln -s  /usr/local/git/bin/git /usr/bin/git
```