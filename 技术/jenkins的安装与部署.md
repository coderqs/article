# Jenkins 的安装与部署
## 准备
### 硬件要求
最低硬件要求  
- 256 MB 内存;
- 1 GB 的驱动器空间（尽管如果将 Jenkins 作为 Docker 容器运行，建议至少使用 10 GB）;

小团队推荐的硬件配置：  
- 4 GB+ 的内存;
- 50 GB+ 的驱动器空间;

更多要求请参考官方的[硬件推荐](https://www.jenkins.io/doc/book/scaling/hardware-recommendations/)  

### 软件要求
#### Java
- 32 位和 64 位版本均支持 Java 8 运行时环境
- 支持 Java 11 运行时环境
  - Java 11 Docker 安装说明包含在“在 Docker 中下载和运行 Jenkins”中
  - 有关其他升级说明，请参阅Java 8 到 Java 11 升级指南
- 不支持 Java 7 及之前版本
- 不支持 Java 9 和 10
- 不支持 Java 12、13、14、15 和 16

这些要求适用于 Jenkins 系统的所有组件，包括 Jenkins 控制器、所有类型的代理、CLI 客户端和其他组件。  

## 安装
### Centos
安装命令:  
``` shell
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
sudo yum install epel-release java-11-openjdk-devel
sudo yum install jenkins
sudo systemctl daemon-reload
sudo systemctl start jenkins
``` 

### Windows
在 [下载 Jenkins](https://www.jenkins.io/download/#downloading-jenkins) 的页面下载对应的 msi 安装包，根据提示安装即可。

### 设置向导
#### 解锁 Jenkins
当第一次访问一个新的 Jenkins 实例时，需要使用系统自动生成的密码来解锁  
- 访问 `http://localhost:8080` (端口默认是
\`8080\`，如果有修改则替换成修改的端口，**要记得在防火墙添加对应的端口**);
    ![jenkins/setup-jenkins-01-unlock-jenkins-page](https://raw.githubusercontent.com/coderqs/wiki_img/f0e6f3affa6530fd03fb41508004601f16de6135/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/setup-jenkins-01-unlock-jenkins-page.jpg) 
- 自动生成的密码在 `/var/lib/jenkins/secrets/` （Windows 在 `C:\Program Files\Jenkins\secrets\`，如果自定义安装路径则在安装的路径中）路径下的 `initialAdminPassword` 文件中; 

#### 使用插件自定义 Jenkins
建议选择 `Install suggested plugins`(建议安装的插件)

## 配置主从节点
Jenkins master 附带 Jenkins 的基本安装，在此配置中，master
处理构建系统的所有任务并分配从节点并将构建发送给从节点以执行作业。

### 设置 Centos 从节点
1.  添加新节点(`Manage Jenkins` -\> `Manage Nodes and Clouds` -\>`新建节点`);  
- 输入节点名称，勾选 `Permanent Agent`;  
    ![新建节点](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E6%96%B0%E5%BB%BA%E8%8A%82%E7%82%B9.PNG)  
- 建议设置 `描述` 和 `标签` 方便管理;  
- 在 `远程工作目录` 设置**从节点机器上 Jenkins 的专用目录**;  
    ![设置远程工作目录](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E6%96%B0%E5%BB%BA%E8%8A%82%E7%82%B9%E8%AE%BE%E7%BD%AE_01.PNG)  
- 启动方式选择 `Launch agents via SSH`，`主机` 中输入从节点的
    ip，`Credentials` 选择对应的凭证(如果没有可以点击旁边的 `添加` 选择 `Jenkins`);  
    ![设置从节点连接信息](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E6%96%B0%E5%BB%BA%E8%8A%82%E7%82%B9%E8%AE%BE%E7%BD%AE_02.PNG)  
- 添加凭证;  
    ![添加凭证](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E6%B7%BB%E5%8A%A0%E5%87%AD%E8%AF%81.PNG)  
- 保存

### 设置 Windows 从节点
**前半部分与 [设置 Centos 从节点](##设置 Centos 从节点) 一致，从第 5 步开始不同**。  
- 启动方式选择
`Launch agent by connecting it the master`(如果没有这项则需要修改 `Configure Global Security`)，勾选 `Use WebSocket`;  
    ![选择启动方式](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E6%96%B0%E5%BB%BAwindows%E8%8A%82%E7%82%B9%E8%AE%BE%E7%BD%AE_01.PNG)  
- 修改 `Manage Jenkins -> Configure Global Security -> 代理`;  
    ![修改 Configure Global Security](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E6%96%B0%E5%BB%BAwindows%E8%8A%82%E7%82%B9%E8%AE%BE%E7%BD%AE_02.PNG)  
- 保存，返回节点列表;  
    ![节点列表](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_%E8%8A%82%E7%82%B9%E5%88%97%E8%A1%A8.PNG)  
- 点击刚刚添加的节点会弹出下面的页面，点击其中的 `agent` 下载文件 `agent.jar`;  
    ![节点信息页面](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/ci_cd/jenkins/Jenkins%E7%9A%84%E5%AE%89%E8%A3%85%E4%B8%8E%E9%83%A8%E7%BD%B2_windows%E8%8A%82%E7%82%B9%E4%BF%A1%E6%81%AF%E9%A1%B5%E9%9D%A2.PNG)  
- 把刚下载的文件放到从节点机器上，并执行页面中的命令(**注：需要 java 环境，要提前装好，版本最好与主节点上的一致**+);  

## 参考资料
[官网 - 安装 Jenkins](https://www.jenkins.io/doc/book/installing/)  
[Jenkins 配置主从节点](https://dzone.com/articles/jenkins-03-configure-master-and-slave)  
[jenkins 配置windows节点实现自动化部署](https://www.cnblogs.com/xiaomifeng0510/p/11848834.html)  
