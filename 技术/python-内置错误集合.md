# Python 内置错误集合

### BaseException—\>所有异常的基类
```
    *SystemExit: 解释器请求退出 \\
    *KeyboardInterrupt: 用户中断执行(输入) \\
    *GeneratorExit: 生成器发生异常通知退出 \\
```

### Exception—\>常规异常的基类
```
    *StopIteration: 迭代器没有更多的值\\
    *StopAsyncIteration: 必须通过异步迭步器''__anext__()''方法引发以停止迭代 \\
```

### ArithmeticError—\>各种运算错误异常的基类
```
    *FloatingPointError: 浮点计算错误 \\
    *OverflowError: 数值运算结果太大无法表示 \\
    *ZeroDivisionError: 除法或模运算的第二个自变量为零 \\
    *AssertionError: assert(断言)语句失败时\\
    *AttributeError: 属性引用或赋值失败\\
    *BufferError: 无法执行与缓存区相关的操作\\
    *EOFError: 当input()函数达到文件结束条件(EOF)而不读取任何数据时
```

### ImportError—\>导入模块或对象失败
```
    *ModuleNotFoundError: 无法找到要导入的模块
```

### LookupError—\>映射或序列上使用的键或索引无效的基类
```
    *IndexError: 序列下标超出范围\\
    *KeyError: 在现有键集中找不到映射(字典)的键\\
    *MemoryError: 操作内存不足
```

### NameError—\>未声明/初始化对象
```
    *UnboundLocalError: 访问未初始化的本地变量
```

### OSError—\>操作系统错误
```
    *BlockingIOError: 操作将阻塞对象设置为非阻塞操作\\
    *ChildProcessError: 子进程上的操作失败
```

### ConnectionError—\>与连接相关的异常的基类
```
    *BrokenPipeError: 另一端关闭时尝试写入管道或试图在已关闭写入的套接字上写入\\
    *ConnectionAbortedError: 等方终止连接尝试\\
    *ConnectionRefusedError: 等端拒绝连接尝试\\
    *ConnectionResetError: 等方重置连接\\
    *FileExistsError: 尝试创建已经存在的文件或目录\\
    *FileNotFoundError: 请求文件或目录但不存在\\
    *InterruptedError: 系统调用被传入信号中断\\
    *IsADirectoryError: 在目录上请求文件操作 (例如os.remove())\\
    *NotADirectoryError: 在非目录上请求目录操作(例如os.listdir())\\
    *PermissionError: 尝试在没有足够访问权限（例如文件系统权限）的情况下运行操作\\
    *ProcessLookupError: 给定的进程不存在\\
    *TimeoutError: 系统功能在系统级别超时\\
    *ReferenceError: weakref.proxy()函数创建的弱引用试图访问已经垃圾回收了的对象
```

### RuntimeError—\>检测到不属于任何其他类别的错误时
```
    *NotImplementedError: 在用户定义的基类中，抽象方法要求派生类重写该方法或者正在开发的类指示仍然需要添加实际实现\\
    *RecursionError: 解释器检测到超出最大递归深度
```

### SyntaxError—\>语法错误的基类

### IndentationError—\>缩进错误
```
    *TabError: Tab和空格混用出现错误\\
    *SystemError: 解释器发现内部错误时\\
    *TypeError: 将操作或功能应用于不合适类型的对象
```

### ValueError—\>操作或函数接收到类型正确但值不合适的参数

### UnicodeError—\>发生与Unicode相关的编码或解码错误
```
    *UnicodeDecodeError: Unicode解码错误\\
    *UnicodeEncodeError: Unicode编码错误\\
    *UnicodeTranslateError: Unicode转码错误
```

### Warning—\>警告类别的基类
```
    *DeprecationWarning: 有关已弃用功能的警告的基类\\
    *PendingDeprecationWarning: 有关不推荐使用功能的警告的基类\\
    *RuntimeWarning: 有关可疑运行时行为的基类\\
    *SyntaxWarning: 有关可疑语法的基类
    *UserWarning: 用户代码生成的警告的基类\\
    *FutureWarning: 有关已弃用功能的警告的基类\\
    *ImportWarning: 用于警告有关模块导入中可能错误的警告的基类\\
    *UnicodeWarning: 与Unicode相关的警告的基类\\
    *BytesWarning: 与bytes和bytearray有关的警告的基类\\
    *ResourceWarning: 与资源使用相关的警告的基类
```

## 参考资料
[绝对干货！一张图整理了 Python 所有内置异常！](https://blog.csdn.net/weixin_44275820/article/details/107779407?utm_medium=distribute.pc_category.none-task-blog-hot-1.nonecase&depth_1-utm_source=distribute.pc_category.none-task-blog-hot-1.nonecase&request_id=)
