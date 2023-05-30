# Gitea 安装与部署
## 准备
### 下载文件
从官网的[下载页面](https://dl.gitea.io/gitea/)中选择与目标平台匹配的文件，复制 URL 并替换以下命令中的 URL：
``` shell
wget -O gitea https://dl.gitea.io/gitea/1.14.5/gitea-1.14.5-linux-amd64 shell
chmod +x gitea
```

### 环境准备
#### 确保已安装 Git
``` shell
git --version
```

#### 创建运行 Gitea 的用户
``` shell
adduser \
   git \
   --system \
   --shell /bin/bash \
   --home /home/git
```

#### 创建所需的目录结构
``` shell
mkdir -p /var/lib/gitea/{custom,data,log}
chown -R git:git /var/lib/gitea/
chmod -R 750 /var/lib/gitea/
mkdir /etc/gitea
chown root:git /etc/gitea
chmod 770 /etc/gitea
``` 
**注意**：
/etc/gitea是具有用户写权限的临时设置，git以便Web安装程序可以写配置文件。安装完成后，建议使用以下命令将权限设置为只读：
``` shell
chmod 750 /etc/gitea
chmod 640 /etc/gitea/app.ini
```

#### 移动 Gitea 二进制文件
``` shell
 chown git:git gitea
 mv gitea /usr/local/bin/gitea
```

## 安装
### 运行
执行命令
``` shell
GITEA_WORK_DIR=/var/lib/gitea/ /usr/local/bin/gitea web -c /etc/gitea/app.ini
```
使用参数 `-p` 可指定自定义端口运行 Gitea

### 网页安装
Gitea 启动后在浏览器中输入对应的 URL，例如：`192.168.1.1:3000` 即可出现安装的页面  
![Gitea 安装页面](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/gitea/gitea_%E5%AE%89%E8%A3%85%E7%95%8C%E9%9D%A2.png)  
这些设置没什么特别难懂的地方，根据提示填写自己的设置保存并安装即可。等安装完成后会自动跳转到用户页面  
![Gitea 用户页面](https://raw.githubusercontent.com/coderqs/wiki_img/master/%E5%B7%A5%E5%85%B7/%E7%BC%96%E7%A8%8B%E5%B7%A5%E5%85%B7/%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6/gitea/gitea_%E7%94%A8%E6%88%B7%E9%A1%B5%E9%9D%A2.png)  
考虑到安全问题，需要修改 `/etc/gitea` 的权限
``` shell
chmod 750 /etc/gitea
chmod 640 /etc/gitea/app.ini
```

### 设置系统服务
将下面的内容复制到
`/etc/systemd/system/gitea.service`，然后将要使用的服务放开注释，例如
MySQL。
``` service
[Unit]
Description=Gitea (Git with a cup of tea)
After=syslog.target
After=network.target
###
# Don't forget to add the database service dependencies
###
#
#Wants=mysql.service
#After=mysql.service
#
#Wants=mariadb.service
#After=mariadb.service
#
#Wants=postgresql.service
#After=postgresql.service
#
#Wants=memcached.service
#After=memcached.service
#
#Wants=redis.service
#After=redis.service
#
###
# If using socket activation for main http/s
###
#
#After=gitea.main.socket
#Requires=gitea.main.socket
#
###
# (You can also provide gitea an http fallback and/or ssh socket too)
#
# An example of /etc/systemd/system/gitea.main.socket
###
##
## [Unit]
## Description=Gitea Web Socket
## PartOf=gitea.service
##
## [Socket]
## Service=gitea.service
## ListenStream=<some_port>
## NoDelay=true
##
## [Install]
## WantedBy=sockets.target
##
###
[Service]
# Modify these two values and uncomment them if you have
# repos with lots of files and get an HTTP error 500 because
# of that
###
#LimitMEMLOCK=infinity
#LimitNOFILE=65535
RestartSec=2s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
# If using Unix socket: tells systemd to create the /run/gitea folder, which will contain the gitea.sock file
# (manually creating /run/gitea doesn't work, because it would not persist across reboots)
#RuntimeDirectory=gitea
ExecStart=/usr/local/bin/gitea web --config /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea
# If you install Git to directory prefix other than default PATH (which happens
# for example if you install other versions of Git side-to-side with
# distribution version), uncomment below line and add that prefix to PATH
# Don't forget to place git-lfs binary on the PATH below if you want to enable
# Git LFS support
#Environment=PATH=/path/to/git/bin:/bin:/sbin:/usr/bin:/usr/sbin
# If you want to bind Gitea to a port below 1024, uncomment
# the two values below, or use socket activation to pass Gitea its ports as above
###
#CapabilityBoundingSet=CAP_NET_BIND_SERVICE
#AmbientCapabilities=CAP_NET_BIND_SERVICE
###
[Install]
WantedBy=multi-user.target
```

## 部署脚本
详见[gitea_deploy#gitea_deploy.sh](/脚本管理/gitea_deploy#gitea_deploy.sh)

## 扩展
### 启用 SSH 支持
禁止 git 用户进行 ssh 登录，但还能使用 ssh url 克隆存储库 A

#### 解决方法
1\. 配置 `/etc/gitea/app.ini` 中的 `SSH_DOMAIN`，例如
``` shell
SSH_DOMAIN = git.domain.tld
```
2\. 配置 ssh 默认情况下，Gitea 将以 user `git`
身份运行，并且此帐户将用于 ssh 存储库访问。要使用 ssh 访问，必须启用
PAM。  
修改文件 `/etc/ssh/sshd_config`
``` shell
...
UsePAM yes
...
```
重启 ssh 服务  

参考自[https://wiki.archlinux.org/title/Gitea#Enable_SSH_Support#Enable SSH Support](https://wiki.archlinux.org/title/Gitea#Enable_SSH_Support#Enable%20SSH%20Support)

## 参考资料
[install-from-binary](https://docs.gitea.io/en-us/install-from-binary/)  
[Run Gitea as Linux service](https://docs.gitea.io/en-us/linux-service/)  
