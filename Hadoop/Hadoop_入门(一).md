## 目录
##### [Hadoop 安装 ](http://www.jianshu.com/p/8e57916790f2)
##### [单点启动&集群启动](http://www.jianshu.com/p/715dc8601065)
##### [访问 HDFS](http://www.jianshu.com/p/d8a5459c9f02)
##### [常用配置](http://www.jianshu.com/p/09bd95bdef0f)
##### [常用命令](http://www.jianshu.com/p/2a13831d0e79)


# Hadoop 安装
## 软件准备
**运行环境**
```
[root@master network-scripts]# lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.2.1511 (Core)
Release:        7.2.1511
Codename:       Core
```

**Hadoop 版本**
```
[root@master ~]# hadoop version
Hadoop 2.7.3
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r baa91f7c6bc9cb92be5982de4719c1c8af91ccff
Compiled by root on 2016-08-18T01:41Z
Compiled with protoc 2.5.0
From source with checksum 2e4ce5f957ea4db193bce3734ff29ff4
This command was run using /usr/local/hadoop/share/hadoop/common/hadoop-common-2.7.3.jar
```

**Java 版本**
```
[root@master ~]# java -version
java version "1.8.0_91"
Java(TM) SE Runtime Environment (build 1.8.0_91-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b14, mixed mode)
```


## CentOS&软件 安装

**CentOS选择基础设施服务器安装**
### 网络配置
```
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eno16777736
TYPE=Ethernet
ONBOOT=yse
IPADDR=192.168.8.150
NETMASK=255.255.255.0
GATEWAY=192.168.8.2
DNS1=8.8.8.8
[root@localhost ~]# cat /etc/sysconfig/network
# Created by anaconda
NETWORKING=yes
GATEWAY=192.168.8.2
```
**修改主机名**
*主机名千万不能有下划线！*
```
[root@localhost ~]# hostnamectl set-hostname node0
```
**重启网络服务**
```
[root@localhost ~]# /etc/init.d/network restart
Restarting network (via systemctl):  [  确定  ]
```

将jdk hadoop安装文件上传至`/usr/local/`

**安装 jdk**
```
[root@localhost local]# rpm -ivh jdk-8u91-linux-x64.rpm
准备中...                          ################################# [100%]
正在升级/安装...
   1:jdk1.8.0_91-2000:1.8.0_91-fcs    ################################# [100%]
Unpacking JAR files...
        tools.jar...
        plugin.jar...
        javaws.jar...
        deploy.jar...
        rt.jar...
        jsse.jar...
        charsets.jar...
        localedata.jar...
        jfxrt.jar...
[root@localhost local]# rpm -qa|grep jdk
jdk1.8.0_91-1.8.0_91-fcs.x86_64
[root@localhost local]# java -version
java version "1.8.0_91"
Java(TM) SE Runtime Environment (build 1.8.0_91-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b14, mixed mode)
```
jdk 默认安装在`/usr/java/`

**安装 hadoop**
```
[root@localhost local]# tar -xvf ./hadoop-2.7.3.tar.gz
[root@localhost local]# mv hadoop-2.7.3 hadoop
[root@localhost local]# cd ./hadoop/etc/hadoop/
[root@localhost hadoop]# pwd
/usr/local/hadoop/etc/hadoop
```
修改 hadoop-env.sh 中的 JAVA_HOME
```
[root@localhost hadoop]# vi hadoop-env.sh
找到 export JAVA_HOME=/usr/java/default
```
修改 core-site.xml 配置文件
```
[root@localhost hadoop]# vi core-site.xml
在文件的末尾加入
<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://node0:9000</value>
	</property>
    <property>
		<name>hadoop.tmp.dir</name>
		<value>/var/hadoop</value>
	</property>
</configuration>
```
[core-site.xml](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/core-default.xml)

fs.defaultFS    namenode位置
hadoop.tmp.dir  文件存放位置，默认在 /tmp/ 目录下

把 hadoop 执行命令加入环境变量
```
[root@localhost hadoop]# vi /etc/profile
在文件末尾加入
export PATH=$PATH:/usr/local/hadoop/bin:/usr/local/hadoop/sbin
[root@localhost hadoop]# source /etc/profile
```

修改 /etc/hosts 文件
```
[root@localhost hadoop]# vi /etc/hosts

192.168.8.150 node0
192.168.8.151 node1
192.168.8.152 node2
192.168.8.153 node3
```

**拍摄快照**
```
给 node0 拍摄快照
复制 3 份
修改 ip 和 hostname ，确保相互之间可 ping 通
```

**格式化 namenode**
```
[root@node0 ~]# hdfs namenode -format
```

**关闭防火墙**
```
[root@node0 tmp]# systemctl stop firewalld
[root@node0 tmp]# systemctl disable firewalld
Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
Removed symlink /etc/systemd/system/basic.target.wants/firewalld.service.
```

**启动 namenode**
```
[root@node0 ~]# hadoop-daemon.sh start namenode
starting namenode, logging to /usr/local/hadoop/logs/hadoop-root-namenode-node0.out
[root@node0 ~]# jps
2849 NameNode
2918 Jps
```
**启动 datanode**
```
[root@node1 ~]# hadoop-daemon.sh start datanode
starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-node1.out
[root@node1 ~]# jps
2756 DataNode
2789 Jps
```

**检测hdfs状态**
```
[root@node0 tmp]# hdfs dfsadmin -report
```

**远程浏览器访问 HDFS**
```
http://192.168.8.150:50070
```

执行到这里 hadoop 集群已经搭建起来


[参考资料 http://mashibing.com/w/](http://mashibing.com/w/)
