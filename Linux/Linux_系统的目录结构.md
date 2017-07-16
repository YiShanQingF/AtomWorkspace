# Linux_系统的目录结构


### Linux目录结构
逻辑上所有的目录只有一个顶点 /(根)，所有目录的起点

>* 应用程序	/usr/bin
>* 数据文件	/usr/share
>* 配置文件	/etc/
>* 启动命令	/etc/init.d

>mount /dev/sdb1 /usr

**目录结构和分区设备没有关联，不同的目录可以跨越不同的磁盘设备或分区**

### 相对路径和绝对路径

### 系统重要文件
> /etc/issue 			系统登录前用户登录显示的信息

> /ect/motd				登录提示

> /etc/passwd			用户信息文件

> /etc/shadow			用户密码

> /etc/group			用户组

> /etc/gshadow			组的密码

> /etc/sudoers			授权文件

> /etc/securetty		设定那些终端可以让root登录

> /etc/login.defs		所有用户登录时的缺省配置

> /etc/syslog.conf		系统日志设置 限C5.X

> /etc/rsyslog.conf		系统日志设置 限C6.X

>* chkconfig --list rsyslog


> /usr/local			自编译安装软件的存放目录

> /usr/src				内核源码存放目录

> /usr/local/sbin		系统全局环境目录，可放置一些不需要加路径执行的脚本

> /usr/share			系统共用的东西存放地，比如：/usr/share/doc和/usr/share/man帮助文件

> /usr/src				内核源码存放目录

> /usr/bin				使用者可执行的 binary file 的目录

> /usr/local/bin		使用者可执行的 binary file 的目录

> /usr/lib				系统函数

> /usr/local/lib		系统函数

> /var					日志文件

> /var/log				系统各种日志存放地

> /var/log/messages		系统信息默认日志文件，重要，按周循环

> /var/log/securetty	登录信息

> /var/log/wtem			login records

>*	last				查看登录历史

>*	lastlog				系统那些用户登录过

> /var/spool			定时任务 crontab 默认路径，按用户名命名的文件

> /var/spool/cron/root

> /var/spool/mail			系统用户邮件存放目录

> /var/spool/clientmqueue	sendmail 临时邮件目录，有很多原因会导致这个目录碎文件很多，导致/var所在的分区 iNode 数量消耗尽，无法写入的情况

> /proc						虚拟目录，是内存的映射,内核和进程的虚拟文件系统目录

> /proc/version				内核版本

> /proc/sys/kernel			系统内核功能

> /proc/sys/net/inv4		修改 proc 的配置是临时生效

> /etc/sysctl.conf			内核参数配置永久生效

> /proc/loadavg				系统负载平均值信息，（系统的繁忙情况，比较准确，但是不够细致系统性能指标），uptime 的结果

> /etc/rc.local				存放开机自启动内容的文件 chkconfig 一般是用来管理 yum/rpm 包安装的服务

[linux目录结构详细介绍](http://yangrong.blog.51cto.com/6945369/1288072 "linux目录结构详细介绍")
