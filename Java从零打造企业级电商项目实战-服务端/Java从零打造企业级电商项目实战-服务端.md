# Java从零打造企业级电商项目实战-服务端

## 环境
一、Linux：centos6.8 64bit http://mirrors.aliyun.com/centos/6.8/isos/x86_64/CentOS-6.8-x86_64-bin-DVD1.iso

二、阿里云软件源配置说明
本教程所用centos：http://mirrors.aliyun.com/help/centos

三、git安装依赖
sudo yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils- MakeMaker

四、nginx安装依赖命令
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel


## 开发环境安装与配置讲解、实操（Linux、Windows）

### Linux 软件配置与学习建议
1、Linux 权限管理之基本权限
http://www.imooc.com/view/481

2、Linux 软件安装管理
http://www.imooc.com/learn/447

3、Linux 达人养成计划
http://www.imooc.com/learn/175
http://www.imooc.com/learn/111

4、Linux 服务管理
http://www.imooc.com/view/537

5、学习 iptables 防火墙
http://www.imooc.com/learn/389

6、版本管理工具介绍- Git 篇
http://www.imooc.com/learn/208

7、Tomcat 基础
http://www.imooc.com/learn/166

8、Maven 基础
http://www.imooc.com/learn/443

9、MySql 基础
http://www.imooc.com/learn/122

### Linux 软件源配置实操


### JDK 安装实操（Linux）
1、清理系统默认自带jdk
  如果安装 centos6.8 时默认安装了例如 openjdk 等，请先执行 rpm -qa | grep jdk 查看已经自带的 jdk ，然后卸载
  sudo yun remove XXX （XXX 为上一命令查询到的结果）

2、赋予权限
  sudo chmod 777 jdk-7u80-linux-x64.rpm

3、安装
  sudo rpm -ivh jdk-7u80-linux-x64.rpm

4、默认安装路径 /usr/java

5、jdk 配置环境变量
  （1）sudo vim /etc/profile
  （2）在文件最下面增加
       export JAVA_HOME=/usr/java/jdk1.7.0_80
       export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
  （3）在 export PATH 中添加 $JAVA_HOME/bin
  （4）保存退出
  （5）使配置生效 source /etc/profile

6、jdk 验证
  执行 java -version


### Tomcat
#### 下载
http://archive.apache.org/dist/tomcat/

#### 解压缩

#### 配置环境变量
  （1）vim /etc/profile

  （2）在最下方增加 export CATALINA_HOME=/developer/apache-tomcat-7.0.73

  （3）保存退出

  （4）使配置生效 source /etc/profile

  （5）配置 UTF-8 字符集

      进入 tomcat 安装的 conf 文件夹，编辑 server.xml
      找到配置 8080 默认端口的位置，在 xml 节点末尾增加 URIEncoding="UTF-8"


### Maven

  1、用 Maven 可以方便创建项目，基于 archetype 可以创建多种类型的 java 项目
  2、Maven 仓库对 jar 包（artifact）进行统一管理，避免 jar 文件的重复拷贝和版本冲突
  3、团队开发，Maven 管理项目的 RELEAS 和 SHAPSHOT 版本，方便多模块（Module）项目的各个模块之间的快速集成


### vsftpd
  vsftpd 是 "very secure FTP deamon" 缩写，是一个完全免费，开源的代码的 ftp 服务器软件
  小巧轻快，安全易用，支持虚拟用户，支持宽带限制等功能

  （1）安装：执行 yum -y install vsftpd  默认配置文件在 /etc/vdftpd/vdftpd.conf

  （2）创建虚拟用户 1、创建 ftp 文件夹:mkdir /ftpfile 2、添加匿名用户:useradd ftpuser -d /ftpfile -s /sbin/nologin 3、修改 ftpfile 权限:chown -R ftpuser.ftpuser /ftpfile 4、重置 ftpuser 密码:passwd ftpuser

  （3）配置：1、cd /etc/vsdftpd  2、vim chroot_list   3、把新增的虚拟用户添加到此配置文件中，后续要引用 4、:wq保存退出  5、vim /etc/selinux/config，修改 SELINUX=disabled  setenforce 0  6、:wq 保存退出 注：验证的时候碰到550拒绝访问执行：setsebool -P ftp_home_dir 1 然后重启 linux 服务器

  （4）vim /etc/vsftpd/vsftpd.conf    http://learning.happymmall.com/vsftpdconfig/

  （5）防火墙配置:vim /etc/sysconfig/iptables

  （6）#vsftpd
      -A INPUT -p TCP --dport 61001:62000 -j ACCEPT

      -A OUTPUT -p TCP --sport 61001:62000 -j ACCEPT

      -A INPUT -p TCP --dport 20 -j ACCEPT

      -A OUTPUT -p TCP --sport 20 -j ACCEPT

      -A INPUT -p TCP --dport 21 -j ACCEPT

      -A OUTPUT -p TCP --sport 21 -j ACCEPT

  （7） 重启 `service vsftpd restart`


### Nginx
    Nginx 是一个轻量级 Web 服务器，也是一款方向代理服务器

    用途：
    1、可以直接支持 Rails 和 PHP 的程序
    2、可以作为 HTTP 方向代理服务器
    3、作为负载均衡服务器
    4、作为邮件服务器
    5、帮助实现前端动静分离

    特点：
    高稳定 高性能 资源占用少 功能丰富  模块化结构 支持热部署

    安装
    （1）安装gcc（yum install gcc）
    （2）安装pcre(yum install pcre-devel)
    （3）安装zlib(yum install zlib zlib-devel)
    （4）安装openssl（yum install openssl openssl-devel）
    （5）下载源码包，选择稳定版本（http://www.nginx.org）  tar -zxvf nginx-1.10.2.tar.gz
    （6）进入nginx目录之后执行 ./configure  注：指定安装目录， --prefix=/usr/nginx 、 whereis nginx （默认安装在 /usr/local/nginx）
    （7）执行 make
    （8）执行 make install

    测试配置文件
      安装路径下 /nginx/sbin/nginx -t

    启动命令
      安装路径下 /nginx/sbin/nginx

    停止命令
      安装路径下 /nginx/sbin/nginx -s stop，或者是 nginx -s quit

    重启命令
      安装路径下 /nginx/sbin/nginx -s reload

    平滑重启
      kill -HUP【Nginx主进程号】

    虚拟域名配置及测试验证
      配置步骤 vim /usr/local/nginx/conf/nginx.conf
        增加 include vhost/*.conf

### MySql

    执行 yum -y install mysql-server

    默认配置文件 /etc/my.cnf

    添加配置，在[mysqld]节点下添加

    default-character-set=utf-8

    character-set-server=utf-8

    自启动配置

      执行：chkconfig mysqld on

      执行：chkconfig --list mysqld 查看（如果2--5位启动on状态即OK）

    防火墙配置

      vi /etc/sysconfig/iptables

      -A INPUT -p tcp -m --dport 3306 -j ACCEPT(将配置添加到防火墙配置中)

      service iptables restart(重启防火墙)
