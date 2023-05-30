# indexmenu
[indexmenu](https://www.dokuwiki.org/plugin:indexmenu)
可以插入完全可自定义的索引或从指定名称空间开始的页面列表。

## 基本语法
```
{{indexmenu>ns[#n] [ns1[#n] ns2[#n] …]  | [js[#theme]] [tsort] … }}
```
**`[]` 内的是可选项**

## 常用的几种语句
### 从根名称空间显示
全部展开
```
{{indexmenu>:}}
```
### 从当前名称空间
仅显示当前级别，不展开任何节点
```
{{indexmenu>.#1}}
```

### 展开当前页面的父名称空
显示父名称空间（“上行链路”）和当前级别，不展开任何节点
```
{{indexmenu>..:#1}}
```
### 展开包含指定的名称空间
以展开包含 wiki 的名称空间为例，语法如下：
```
{{indexmenu>:wiki#1|js}}
```
### 不显示指定的名字空间
以不显示 `users` 名字空间为例，语法如下：
```
{{indexmenu>.:#1|js skipns=/^users$/}}
```

### 不显示指定页面
以不显示 `start` 和 `sidebar` 页面为例，语法如下：
```
{{indexmenu>.:#1|js skipfile+/(^|:)(start|sidebar)$/}}
```

## 遇到的问题
### Indexmenu id conflict
这个问题是因为页面中有多个 Indexmenu 生成的索引。  
我遇见这个问题是因为我打开了 `sidebar` 页面，页面与侧边栏都渲染了一遍导致的，并不影响使用，关掉 `sidebar` 页面就行了。
