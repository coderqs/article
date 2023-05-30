# C++ 编译相关的错误整理

## undefined reference to XXX
这是非常常见的一个问题，原因差不多有以下三点：
1.  编译器找不到定义了XXX的文件;  
2.  定义了XXX的文件，由于函数修饰的原因里面没有想要的XXX符号;  
3.  找到了想要的符号，但是该符号是隐藏属性，不能链接使用;  
**详细的解释与解决方法请查看参考资料**。  

### 参考资料
["undefined reference to XXX"问题总结](https://zhuanlan.zhihu.com/p/81681440)  

## expected primary-expression before xx token
这个 `xx` 指的是一半都是运算符，比如 `++`，`—` 等  

#### 示例
`expected primary-expression before ')' token`

#### 原因
把类型(type)当成变量来用了(variable)  

### 参考资料
[expected primary-expression before xx token 错误处理](https://www.cnblogs.com/MartinLwx/p/12533140.html)

## expected unqualified-id before '(' token
#### 原因
头文件的引用顺序引起的，只需要调整引用该头文件的其他文件在报错的 .cpp 文件中的引用顺序即可，一般将该文件或者引用该头文件的头文件置于自定义头文件的前面。

### 参考资料
[linux下编译复数类型引发的错误：expected unqualified-id before '(' token](https://www.cnblogs.com/yeahgis/archive/2012/12/25/2831932.html)  
[expected unqualified-id before '(' token](https://blog.csdn.net/haidonglin/article/details/78810264)

## DSO missing from command line 
出现的场景是：  
- 我们有一个shared libA中，定义了函数foo()  
- 另一个静态库libB显示地链接了libA  
- 一个可执行文件bin_c显示地链接了libA  

### 原因
不会自动递归链接动态库，需要手动指明。 

### 参考资料 
[DSO missing from command line原因及解决办法](https://segmentfault.com/a/1190000002462705?utm_source=tag-newest\)

## error: explicit qualification in declaration of xxx
#### 示例
```
namespace threedog {
    void threedog::func(int i);
}
```
上面这种写法编译时就会报错误信息 `error: explicit qualification in declaration of 'threedog' `

#### 原因
已经在命名空间中，却又在声明中加上了限定符。去掉错误信息中提示的限定符即可。

### 参考资料
[C++编译错误 ：error: explicit qualification in declaration of xxx](https://blog.csdn.net/Three_dog/article/details/96133220)  
