# SVN 忽略指定的文件或目录
和 Git 中的 `.gitignore` 的功能基本一致。

## 设置方法
1. 在项目下创建一个名为 `.svnignore` 的文件（svn 对这个没有要求，文件名可任意起，但是为了看起来规范所以叫这个）;  
2. 在文件 `.svnignore` 中添加要忽略的文件或目录，语法与 `.gitignore` 基本一致，目前发现有以下不同点;  
    - svn忽略的文件夹名称后面可以不用斜杠.  
    - svn忽略的文件列表，换行符不能是Windows的换行符(`\r\n`)，必须是 Unix 换行符(`\n`)，可以使用编辑软件替换掉(例如：Notepad++).  
3. 在 svn 跟踪的项目目录下点击右键 -\> `TortoiseSVN` -\> `Properties` 会弹出窗口;  
    ![打开properties选项](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/svn/%E6%89%93%E5%BC%80properties%E9%80%89%E9%A1%B9.jpg)  
4. 点击窗口右下角的 `New...` 选择 `Other`，在弹出的窗口中 `Property name` 选择 `svn:global-ignores`;  
    ![选择other](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/svn/%E9%80%89%E6%8B%A9other.jpg)  
5. 点击 `Load...` 选择刚刚准备好的 `.svnignore` 文件，点击 OK 即可。（**不要选择 `Apply property recursively` 这一项**）;  
    ![选择global_ignores](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/svn/%E9%80%89%E6%8B%A9global_ignores.jpg)  

## 参考资料
[【全网独家】SVN 实现和 Git .gitignore一样的全局忽略文件和文件夹](https://zhuanlan.zhihu.com/p/371201105)  
