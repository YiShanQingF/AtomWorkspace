# CentOS安装JDK

## rpm包安装
### 下载rpm包

1. [jdk下载地址](http://www.oracle.com/technetwork/java/javase/downloads/index.html  "jdk下载地址")
2. 下载完成后将JDK上传至/root （其它地方也行）

jdk-7u79-linux-x64.rpm

### 安装jdk
```
[root@node1 ~]# cd /home
[root@node1 ~]# rpm -ivh jdk-7u79-linux-x64.rpm     #安装jdk
Preparing...                ########################################### [100%]
   1:jdk                    ########################################### [100%]
Unpacking JAR files...
        rt.jar...
        jsse.jar...
        charsets.jar...
        tools.jar...
        localedata.jar...
        jfxrt.jar...

[root@node1 ~]# rpm -qi jdk       #查看JDK安装详细信息
Name        : jdk                          Relocations: /usr/java
Version     : 1.7.0_79                          Vendor: Oracle Corporation
Release     : fcs                           Build Date: Sat 11 Apr 2015 02:57:06 AM CST
Install Date: Sun 28 Aug 2016 10:20:42 PM CST      Build Host: sc11137383.us.oracle.com
Group       : Development/Tools             Source RPM: jdk-1.7.0_79-fcs.src.rpm
Size        : 219409562                        License: http://java.com/license
Signature   : (none)
Packager    : Java Software <jre-comments@java.sun.com>
URL         : URL_REF
Summary     : Java Platform Standard Edition Development Kit
Description :
The Java Platform Standard Edition Development Kit (JDK) includes both
the runtime environment (Java virtual machine, the Java platform classes
and supporting files) and development tools (compilers, debuggers,
tool libraries and other tools).

The JDK is a development environment for building applications, applets
and components that can be deployed with the Java Platform Standard
Edition Runtime Environment.

# 从上面可以看到JDK安装在 /usr/java 中还有版本以及其他的各种信息
```

### 配置环境变量

```
[root@node1 jdk1.7.0_79]# pwd
/usr/java/jdk1.7.0_79
[root@node1 jdk1.7.0_79]# vi /etc/profile     #系统的环境
#打开后，在文档最下方加上以下环境变量配置代码：

export JAVA_HOME=/usr/java/jdk1.7.0_79
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

# 注意：export JAVA_HOME=/usr/java/jdk1.7.0_79  由于安装的JDK的版本不同 JAVA_HOME 的路径也有所不同

[root@node1 jdk1.7.0_79]# source /etc/profile     # 使配置文件生效
```

### 检查JDK是否安装成功
```
[root@node1 jdk1.7.0_79]# java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
# 如果看到JVM版本及相关信息，即安装成功！
```

### 卸载jdk

1. 检查系统是否安装jdk

```
[root@node1 jdk1.7.0_79]# rpm -qa | grep jdk
jdk-1.7.0_79-fcs.x86_64
[root@node1 jdk1.7.0_79]# rpm -e jdk-1.7.0_79-fcs.x86_64
```
------

## 源码安装
### 解压JDK
```
[root@node1 ~]# mkdir -p /usr/lib/java
[root@node1 ~]# tar -zxvf jdk-7u79-linux-x64.tar.gz -C /usr/lib/java/
```

### 配置
```
[root@node1 jdk1.7.0_79]# vi /etc/profile     #系统的环境
#打开后，在文档最下方加上以下环境变量配置代码：

export JAVA_HOME=/usr/java/jdk1.7.0_79
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

[root@node1 jdk1.7.0_79]# source /etc/profile
[root@node1 jdk1.7.0_79]# java -version
java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)

```
