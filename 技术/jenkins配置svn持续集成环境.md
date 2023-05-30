# Jenkins 配置 SVN 持续集成环境
## 准备
安装 Jenkins 插件 Subversion

## 配置
1.  将 SVN 的用户名和密码添加到[凭证](/工具/编程工具/ci_cd/jenkins/)中;
2.  创建一个新的任务;
    1.  勾选 `General` 中的 `限制项目的运行节点` 并指定节点(无需要可以跳过这步);
    2.  `源码管理` 中勾选 `Subversion`，在 `Repository URL` 中添加仓库的地址，`Credentials` 选择第 1 步中配置的凭证;  
        ![jenkins配置svn持续发布任务_源码管理](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/jenkins%E9%85%8D%E7%BD%AEsvn%E6%8C%81%E7%BB%AD%E5%8F%91%E5%B8%83%E4%BB%BB%E5%8A%A1_%E6%BA%90%E7%A0%81%E7%AE%A1%E7%90%86.PNG)  
    3. 勾选 `构建触发器` 中的 `Build periodically`(控制任务的构建周期) 和 `Poll SCM`(检查 SVN 是否有新的提交的周期)，下图示例是每 30 分钟构建一次任务，5 分钟检查一次 SVN 是否更新;  
        ![jenkins配置svn持续发布任务_构建触发器](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/jenkins%E9%85%8D%E7%BD%AEsvn%E6%8C%81%E7%BB%AD%E5%8F%91%E5%B8%83%E4%BB%BB%E5%8A%A1_%E6%9E%84%E5%BB%BA%E8%A7%A6%E5%8F%91%E5%99%A8.PNG)  
    4. 勾选 `构建环境` 中的 `Delete workspace before build starts`(无需要可以跳过这步);
    5. `构建 -> 增加构建步骤` 中选择准备使用的方式(一般 Linux 环境使用 `Execute shell`，Windows 使用 `Execute Windows batch command`);

## 扩展
### windows 构建方法
在 Windows 下 Jenkins 自带的构建步骤是使用 Windows 批处理脚本，想要编译 VS 的工程会比较麻烦，在这里推荐使用插件 `MSBuild`。

#### 配置 MSBuild
1.  安装插件 MSBuild;  
2.  安装完成后在 \`Global Tool Configuration -\> MSBuild -\> 新增 MSBuild\` 中添加 MSBuild.exe 的路径(默认在`C:\\WINDOWS\\Microsoft.NET\\Framework\\\[version\].` 下，如果不是默认目录自行搜索)并保存;  
    ![设置 MSBuild](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/jenkins%E9%85%8D%E7%BD%AEsvn%E6%8C%81%E7%BB%AD%E5%8F%91%E5%B8%83%E4%BB%BB%E5%8A%A1_%E8%AE%BE%E7%BD%AEMSBuild.PNG)  
3. 创建任务，任务的配置与[#配置](#配置)中创建的步骤基本一致，不同的地方在于最后一步 `构建` 选择的是 `Build a Visiual Studio project or solution using MSBuild`;  
4. `MSBuild Version`
5. `MSBuild Build File`: `.sln` 或 `.proj` 的路径(不知道他的起始路径是哪里，这里就直接填的绝对路径);  
    1. `Command Line Arguments`: MSBuild 的命令行参数，这里提供两个实用的参数，详细的请参考参考资料;  
    2. `-t:project_name`: 生成 `project_name` 项目;  
    3. `-p:Platform=x64`: 生成 x64 版本;  
    4. `-p:Configuration=Release`: 生成 Release 版本;  

## 参考资料
[【手把手】10分钟搭建Jenkins+SVN持续集成环境](https://zhuanlan.zhihu.com/p/145361830)  
[jenkins_windows(七)：SVN自动触发项目构建的配置](https://blog.csdn.net/kongsuhongbaby/article/details/100170537)  
[Jenkins 配置svn自动部署](https://blog.csdn.net/Jasonliujintao/article/details/70812639)  
[命令行上的 MSBuild - C++](https://docs.microsoft.com/en-us/cpp/build/msbuild-visual-cpp?view=msvc-160)  
[MSBuild 命令行参考](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-command-line-reference?view=vs-2019#arguments)  
[使用msbuild指定解决方案的项目文件](https://www.it-swarm.cn/zh/build/%E4%BD%BF%E7%94%A8msbuild%E6%8C%87%E5%AE%9A%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E7%9A%84%E9%A1%B9%E7%9B%AE%E6%96%87%E4%BB%B6/1070142712/)  
