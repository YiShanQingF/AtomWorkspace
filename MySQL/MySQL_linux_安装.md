# Linux 安装 MySQL

### 准备
**运行环境**
```
[root@dbmysql ~]# lsb_release -a
LSB Version:    :base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID: CentOS
Description:    CentOS release 6.7 (Final)
Release:        6.7
Codename:       Final
```
**下载安装包**

[下载地址 http://dev.mysql.com/downloads/mysql/#downloads](http://dev.mysql.com/downloads/mysql/#downloads )

*安装包根据自己环境选择*

*mysql-5.6.35-linux-glibc2.5-x86_64.tar.gz*


### 检查是否安装
```
[root@dbmysql ~]# rpm -qa|grep -i mysql
mysql-libs-5.1.61-4.el6.x86_64
```
*可见已经安装了库文件，应该先卸载，不然会出现覆盖错误。注意卸:载时使用了--nodeps选项，忽略了依赖关系*
```
[root@dbmysql ~]# rpm -e mysql-libs-5.1.61-4.el6.x86_64 --nodeps
```


### 添加mysql组和mysql用户
**用于设置mysql安装目录文件所有者和所属组**
```
[root@dbmysql ~]# groupadd mysql
[root@dbmysql ~]# useradd -r -g mysql mysql
```
*useradd -r参数表示mysql用户是系统用户，不可用于登录系统*

### 解压文件
**将二进制文件解压到指定的安装目录，这里指定为/usr/local**

**将下载的安装文件上传到/usr/local**
```
[root@dbmysql ~]# cd/usr/local/
[root@dbmysql ~]# tar -zxvf mysql-5.6.35-linux-glibc2.5-x86_64.tar.gz
[root@dbmysql ~]# rm -rf mysql-5.6.35-linux-glibc2.5-x86_64.tar.gz
[root@dbmysql ~]# ln -s mysql-5.5.29-linux2.6-x86_64 mysql
```
**修改所属的组和用户**
```
[root@dbmysql ~]# cd mysql
[root@dbmysql ~]# chown -R mysql:mysql ./
```

### 安装

```
[root@dbmysql ~]# ./scripts/mysql_install_db --user=mysql
```
*修改当前目录拥有者为root用户*
```
[root@dbmysql ~]# chown -R root:root ./
```
*修改当前data目录拥有者为mysql用户*
```
[root@dbmysql ~]# chown -R mysql:mysql data
```
**到此数据库安装完毕**

### 将mysql服务加入开机自启动项

*首先需要将scripts/mysql.server服务脚本复制到/etc/init.d/，并重命名为mysql*
```
[root@dbmysql ~]# cp ./support-files/mysql.server /etc/init.d/mysql
```
*通过chkconfig命令将mysqld服务加入到自启动服务项中*
```
[root@dbmysql ~]# chkconfig --add mysql
chkconfig --list mysql
mysql           0:关闭  1:关闭  2:启用  3:启用  4:启用  5:启用  6:关闭
```
**重启系统，mysqld就会自动启动了**

**检查mysql是否启动成功**
```
[root@dbmysql ~]# netstat -anp|grep mysql
tcp        0      0 :::3306                     :::*                        LISTEN      2072/mysqld         
unix  2      [ ACC ]     STREAM     LISTENING     15269  2072/mysqld         /tmp/mysql.sock
[root@dbmysql ~]# ps -ef|grep mysql
root       1975      1  0 22:43 ?        00:00:00 /bin/sh /usr/local/mysql/bin/mysqld_safe --datadir=/usr/local/mysql/data --pid-file=/usr/local/mysql/data/dbmysql.pid
mysql      2072   1975  0 22:43 ?        00:00:00 /usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql --log-error=/usr/local/mysql/data/dbmysql.err --pid-file=/usr/local/mysql/data/dbmysql.pid
root       2280   2249  0 22:45 pts/0    00:00:00 grep mysql
```

### 修改 root 密码
```
[root@dbmysql ~]# mysql -u root
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 5.6.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```
**设置主机名为'localhost' 的登录密码**
```
mysql> set password for root@localhost=password('root');
mysql> exit
```
**用密码登录**
```
[root@dbmysql ~]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 5.6.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```
**开放远程登录权限**
```
mysql> grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
mysql> flush privileges;
```

### 问题

**如果遇到登录不上可以试试以下方法**
```
[root@dbmysql bin]# /etc/init.d/mysql stop
Shutting down MySQL....[确定]
[root@dbmysql bin]# /usr/local/mysql/bin/mysqld_safe --user=mysql --skip-grant-tables --skip-networking &
[1] 2393
[root@dbmysql bin]# 170325 23:12:19 mysqld_safe Logging to '/usr/local/mysql/data/dbmysql.err'.
170325 23:12:19 mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql/data

[root@dbmysql bin]# mysql -u root mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.6.35 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> UPDATE user SET Password=PASSWORD('123456') where USER='root'
    -> FLUSH PRIVILEGES
mysql> quit
```
*以安全模式启动，修改root用户密码*

### 防火墙

**关闭防火墙对 3306 端口的拦截**
```
$ iptables -L -n | grep 3306
$ firewall-cmd --add-port=3306/tcp --permanent
$ firewall-cmd --reload
以上是centos7方式，如果遇到centos6，请用下面的命令：
$ iptables -I INPUT -p tcp -m tcp --dport 3306 -j ACCEPT
$ service iptables save 或者 /etc/rc.d/init.d/iptables save
重启
service iptables restart
查看
/etc/init.d/iptables status
```


[mysql用户权限 http://www.cnblogs.com/llsun/archive/2013/08/06/3240963.html](http://www.cnblogs.com/llsun/archive/2013/08/06/3240963.html)

[参考资料 http://www.111cn.net/database/mysql/44142.htm](http://www.111cn.net/database/mysql/44142.htm)
[参考资料 http://blog.csdn.net/superchanon/article/details/8546254/](http://blog.csdn.net/superchanon/article/details/8546254/)
