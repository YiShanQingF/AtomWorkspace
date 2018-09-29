# Git 源码安装 & Git 服务器搭建

## Git 源码安装
### 安装依赖软件
```
--安装gcc
yum install gcc
--安装g++
yum install gcc-c++

--安装编译所需的包
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install gcc perl-ExtUtils-MakeMaker

```

### 安装 Git
```
--解压源码包 安装git
tar zxvf git-2.18.0.tar.gz
./configure --prefix=/usr/local/git-2.18.0
make && make install

-- 创建软连接
ln -s /usr/local/git-2.18.0/bin/git-upload-pack /usr/bin/git-upload-pack 

-- 配置环境变量
vi /etc/profile
GIT_HOME=/usr/local/git-2.18.0

PATH=$PATH:$GIT_HOME/bin
```

## Git 服务器搭建

[服务器上的 Git - 架设服务器](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E6%9E%B6%E8%AE%BE%E6%9C%8D%E5%8A%A1%E5%99%A8)

[搭建Git服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)

### 公钥和私钥
```
客户端创建公钥和私钥
ssh-keygen
```
### 搭建环境
```
创建git用户
adduser git
切换git用户
su git
变换到git根目录
cd

mkdir .ssh
cd .ssh
将客户端的公钥(id_rsa.pub)写入服务器 ~/.ssh/authorized_keys
vi authorized_keys

修改文件权限
chmod 700 .ssh
chmod 600 .ssh/authorized_keys
```

### 初始化仓库
```
初始化一个空仓库
在 gitrepo 目录下执行 (在/home/git/gitrepo 路径下创建 gittest 的仓库)
git init --bare gittest.git

[git@dbmysql gitrepo]$ git init --bare gittest.git
已初始化空的 Git 仓库于 /home/git/gitrepo/gittest.git/

仓库路径
git@192.168.8.157:/home/git/gitrepo/gittest.git/
```

### 限制 git 活动范围
```
你可以用 Git 自带的 git-shell 工具限制 git 用户的活动范围。只要把它设为 git 用户登入的 shell，那么该用户就无法使用普通的 bash 或者 csh 什么的 shell 程序。编辑 /etc/passwd 文件：
vim /etc/passwd
git:x:500:501::/home/git:/bin/bash
把 bin/sh 改为 /usr/bin/git-shell （或者用 which git-shell 查看它的实际安装路径）。该行修改后的样子如下：
git:x:500:501::/home/git:/usr/local/git-2.18.0/bin/git-shell

现在 git 用户只能用 SSH 连接来推送和获取 Git 仓库，而不能直接使用主机 shell。尝试普通 SSH 登录的话，会看到下面这样的拒绝信息：
[root@dbmysql gittest]# su git
fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
```


