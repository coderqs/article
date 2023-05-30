# errno 错误码说明 
关于 errno 的相关定义都在头文件 `/usr/include/asm/errno.h` 中，下面是从文件中拷出来方便查询 
```
#define EPERM 1 /* Operation not permitted */操作不允许

#define ENOENT 2 /\* No such file or directory \*/文件/路径不存在

#define ESRCH 3 /\* No such process \*/进程不存在

#define EINTR 4 /\* Interrupted system call \*/中断的系统调用

#define EIO 5 /\* I/O error \*/I/O错误

#define ENXIO 6 /\* No such device or address \*/设备/地址不存在

#define E2BIG 7 /\* Arg list too long \*/参数列表过长

#define ENOEXEC 8 /\* Exec format error \*/执行格式错误

#define EBADF 9 /\* Bad file number \*/错误文件编号

#define ECHILD 10 /\* No child processes \*/子进程不存在

#define EAGAIN 11 /\* Try again \*/重试

#define ENOMEM 12 /\* Out of memory \*/内存不足

#define EACCES 13 /\* Permission denied \*/无权限

#define EFAULT 14 /\* Bad address \*/地址错误

#define ENOTBLK 15 /\* Block device required \*/需要块设备

#define EBUSY 16 /\* Device or resource busy \*/设备或资源忙

#define EEXIST 17 /\* File exists \*/文件已存在

#define EXDEV 18 /\* Cross-device link \*/跨设备链路

#define ENODEV 19 /\* No such device \*/设备不存在

#define ENOTDIR 20 /\* Not a directory \*/路径不存在

#define EISDIR 21 /\* Is a directory \*/是路径

#define EINVAL 22 /\* Invalid argument \*/无效参数

#define ENFILE 23 /\* File table overflow \*/文件表溢出

#define EMFILE 24 /\* Too many open files \*/打开的文件过多

#define ENOTTY 25 /\* Not a typewriter \*/非打字机

#define ETXTBSY 26 /\* Text file busy \*/文本文件忙

#define EFBIG 27 /\* File too large \*/文件太大

#define ENOSPC 28 /\* No space left on device \*/设备无空间

#define ESPIPE 29 /\* Illegal seek \*/非法查询

#define EROFS 30 /\* Read-only file system \*/只读文件系统

#define EMLINK 31 /\* Too many links \*/链接太多

#define EPIPE 32 /\* Broken pipe \*/管道破裂

#define EDOM 33 /\* Math argument out of domain of func
\*/参数超出函数域

#define ERANGE 34 /\* Math result not representable \*/结果无法表示

#define EDEADLK 35 /\* Resource deadlock would occur
\*/资源将发生死锁

#define ENAMETOOLONG 36 /\* File name too long \*/文件名太长

#define ENOLCK 37 /\* No record locks available \*/没有可用的记录锁

#define ENOSYS 38 /\* Function not implemented \*/函数未实现

#define ENOTEMPTY 39 /\* Directory not empty \*/目录非空

#define ELOOP 40 /\* Too many symbolic links encountered
\*/遇到太多符号链接

#define EWOULDBLOCK EAGAIN /\* Operation would block \*/操作会阻塞

#define ENOMSG 42 /\* No message of desired type
\*/没有符合需求类型的消息

#define EIDRM 43 /\* Identifier removed \*/标识符已删除

#define ECHRNG 44 /\* Channel number out of range
\*/通道编号超出范围

#define EL2NSYNC 45 /\* Level 2 not synchronized \*/level2不同步

#define EL3HLT 46 /\* Level 3 halted \*/3级停止

#define EL3RST 47 /\* Level 3 reset \*/3级重置

#define ELNRNG 48 /\* Link number out of range \*/链接编号超出范围

#define EUNATCH 49 /\* Protocol driver not attached
\*/协议驱动程序没有连接

#define ENOCSI 50 /\* No CSI structure available
\*/没有可用的CSI结构

#define EL2HLT 51 /\* Level 2 halted \*/2级停止

#define EBADE 52 /\* Invalid exchange \*/无效交换

#define EBADR 53 /\* Invalid request descriptor \*/无效请求描述

#define EXFULL 54 /\* Exchange full \*/交换完全

#define ENOANO 55 /\* No anode \*/无阳极

#define EBADRQC 56 /\* Invalid request code \*/无效请求码

#define EBADSLT 57 /\* Invalid slot \*/无效插槽

#define EDEADLOCK EDEADLK

#define EBFONT 59 /\* Bad font file format \*/错误的字体文件格式

#define ENOSTR 60 /\* Device not a stream \*/设备不是流

#define ENODATA 61 /\* No data available \*/无数据

#define ETIME 62 /\* Timer expired \*/计时器到期

#define ENOSR 63 /\* Out of streams resources \*/流资源不足

#define ENONET 64 /\* Machine is not on the network
\*/机器不在网络上

#define ENOPKG 65 /\* Package not installed \*/包未安装

#define EREMOTE 66 /\* Object is remote \*/对象是远程

#define ENOLINK 67 /\* Link has been severed \*/链接正在服务中

#define EADV 68 /\* Advertise error \*/广告错误

#define ESRMNT 69 /\* Srmount error \*/？

#define ECOMM 70 /\* Communication error on send
\*/发送过程中通讯错误

#define EPROTO 71 /\* Protocol error \*/协议错误

#define EMULTIHOP 72 /\* Multihop attempted \*/多跳尝试

#define EDOTDOT 73 /\* RFS specific error \*/RFS特定错误

#define EBADMSG 74 /\* Not a data message \*/不是数据类型消息

#define EOVERFLOW 75 /\* Value too large for defined data type
\*/对指定的数据类型来说值太大

#define ENOTUNIQ 76 /\* Name not unique on network
\*/网络上名字不唯一

#define EBADFD 77 /\* File descriptor in bad state
\*/文件描述符状态错误

#define EREMCHG 78 /\* Remote address changed \*/远程地址改变

#define ELIBACC 79 /\* Can not access a needed shared library
\*/无法访问需要的共享库

#define ELIBBAD 80 /\* Accessing a corrupted shared library
\*/访问损坏的共享库

#define ELIBSCN 81 /\* .lib section in a.out corrupted
\*/库部分在a.out损坏

#define ELIBMAX 82 /\* Attempting to link in too many shared
libraries \*/试图链接太多的共享库

#define ELIBEXEC 83 /\* Cannot exec a shared library directly
\*/不能直接运行共享库

#define EILSEQ 84 /\* Illegal byte sequence \*/非法字节序

#define ERESTART 85 /\* Interrupted system call should be restarted
\*/应重新启动被中断的系统调用

#define ESTRPIPE 86 /\* Streams pipe error \*/流管错误

#define EUSERS 87 /\* Too many users \*/用户太多

#define ENOTSOCK 88 /\* Socket operation on non-socket
\*/在非套接字上进行套接字操作

#define EDESTADDRREQ 89 /\* Destination address required
\*/需要目的地址

#define EMSGSIZE 90 /\* Message too long \*/消息太长

#define EPROTOTYPE 91 /\* Protocol wrong type for socket
\*/错误协议类型

#define ENOPROTOOPT 92 /\* Protocol not available \*/协议不可用

#define EPROTONOSUPPORT 93 /\* Protocol not supported \*/不支持协议

#define ESOCKTNOSUPPORT 94 /\* Socket type not supported
\*/不支持套接字类型

#define EOPNOTSUPP 95 /\* Operation not supported on transport
endpoint \*/操作上不支持传输端点

#define EPFNOSUPPORT 96 /\* Protocol family not supported
\*/不支持协议族

#define EAFNOSUPPORT 97 /\* Address family not supported by protocol
\*/协议不支持地址群

#define EADDRINUSE 98 /\* Address already in use \*/地址已被使用

#define EADDRNOTAVAIL 99 /\* Cannot assign requested address
\*/无法分配请求的地址

#define ENETDOWN 100 /\* Network is down \*/网络已关闭

#define ENETUNREACH 101 /\* Network is unreachable \*/网络不可达

#define ENETRESET 102 /\* Network dropped connection because of
reset \*/网络由于复位断开连接

#define ECONNABORTED 103 /\* Software caused connection abort
\*/软件导致连接终止

#define ECONNRESET 104 /\* Connection reset by peer
\*/连接被对方复位

#define ENOBUFS 105 /\* No buffer space available
\*/没有可用的缓存空间

#define EISCONN 106 /\* Transport endpoint is already connected
\*/传输端点已连接

#define ENOTCONN 107 /\* Transport endpoint is not connected
\*/传输端点未连接

#define ESHUTDOWN 108 /\* Cannot send after transport endpoint
shutdown \*/传输端点关闭后不能在发送

#define ETOOMANYREFS 109 /\* Too many references: cannot splice
\*/太多的引用：无法接合

#define ETIMEDOUT 110 /\* Connection timed out \*/连接超时

#define ECONNREFUSED 111 /\* Connection refused \*/连接被拒绝

#define EHOSTDOWN 112 /\* Host is down \*/主机已关闭

#define EHOSTUNREACH 113 /\* No route to host \*/无法路由到主机

#define EALREADY 114 /\* Operation already in progress
\*/操作已在进程中

#define EINPROGRESS 115 /\* Operation now in progress
\*/进程中正在进行的操作

#define ESTALE 116 /\* Stale NFS file handle \*/

#define EUCLEAN 117 /\* Structure needs cleaning \*/

#define ENOTNAM 118 /\* Not a XENIX named type file \*/

#define ENAVAIL 119 /\* No XENIX semaphores available \*/

#define EISNAM 120 /\* Is a named type file \*/

#define EREMOTEIO 121 /\* Remote I/O error \*/

#define EDQUOT 122 /\* Quota exceeded \*/

#define ENOMEDIUM 123 /\* No medium found \*/

#define EMEDIUMTYPE 124 /\* Wrong medium type \*/ 
```
