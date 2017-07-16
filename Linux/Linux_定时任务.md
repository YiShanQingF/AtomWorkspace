# Linux_crond定时任务

**Crond**
```
Crond 是 Linux 系统中用来定期执行命令和或者指定程序任务的一种服务或者软件

一般安装C5/C6系统时，crond默认存在

Crond 服务默认每分钟，会检查系统中是否有需要执行的定时任务，如果有系统会根据定义好的规则来执行定时任务


秒级任务：

crond无能为力

自己写守护进程shell循环

Quartz可以实现秒级任务

```

### Linux定时任务分类
#### 系统自身定期执行的任务

#### 用户执行的定时任务


### Linux的定时任务分类

#### at适合执行一次就结束的调度任务
```
[root@node1 ~]# chkconfig --list atd
atd             0:off   1:off   2:off   3:on    4:on    5:on    6:off
```

#### anacron 适合于非7*24小时开机的服务准备的，开机执行的
```
检测停机期间没有执行的任务，在开机后一次执行一遍


```

#### crond

```
服务默认每分钟，会检查系统中是否有需要执行的定时任务，如果有系统会根据定义好的规则来执行定时任务

[root@node1 ~]# chkconfig --list crond
crond           0:off   1:off   2:on    3:on    4:on    5:on    6:off


crond是一个定时任务的守护进程，crontab是用户用来设定定时任务规则的命令
```

### crontab

![](img-Linux定时任务\2016-07-08_085640.png)

**crontab -e 编辑的文件为 /var/spool/cron/root**

```
head -1 /var/spool/cron/root

```


|文件			  |说明		|
|:--:			  |:-- 	|
|/etc/cron.deny	  |	该文件中所列用户不允许使用 crontab 命令		|
|/etc/cron.allow  |	该文件中所列用户允许是crontab 命令，优先于/etc/cron.deny		|
|/var/spool/cron/ |	所用用户 crontab配置文件默认都存放在此目录，文件以用户名命名	|


### 定时任务指令使用格式

**用户的定时任务**6列

**系统的定时任务**8列
```
[root@node1 cron]# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```


**基本格式**
```
[root@node1 ~]# crontab -e
0 9 * * 6,7 /bin/sh /server/scripts/printDate.sh >/dev/null 2>&1
[root@node1 ~]# cat /var/log/cron
Jul  8 04:29:01 node1 CROND[2731]: (root) CMD (/root/shan.sh)
Jul  8 04:30:01 node1 CROND[2739]: (root) CMD (/root/shan.sh)
Jul  8 04:30:01 node1 CROND[2740]: (root) CMD (/usr/lib/sa/sa1 1 1)
Jul  8 04:31:01 node1 CROND[2748]: (root) CMD (/root/shan.sh)
Jul  8 04:32:01 node1 CROND[2753]: (root) CMD (/root/shan.sh)
Jul  8 04:33:01 node1 CROND[2762]: (root) CMD (/root/shan.sh)
Jul  8 04:34:01 node1 CROND[2768]: (root) CMD (/root/shan.sh)
Jul  8 04:35:01 node1 CROND[2773]: (root) CMD (/root/shan.sh)
Jul  8 04:36:01 node1 CROND[2779]: (root) CMD (/root/shan.sh)
Jul  8 04:37:01 node1 CROND[2784]: (root) CMD (/root/shan.sh)

```

01 \* \* \* \* cmd

cmd 为要执行的命令或脚本，例：/bin/sh	/server/scripts/shan.sh

每列之间必须要有一个空格

\* 表示任意时间，实际就是“每”时间的意思

分位上的*就等价于每分钟

**-**表示分隔符，表示一个时间范围，如17-19点，00 17-19 \* \* \* cmd，每天的17,18,19点的00分执行任务

**,**表示分隔时段的意思 30 17,18,19 \* \* \* cmd 表示每天的17,18,19点的30分执行任务

**/n**代表数字，\*/10 \* \* \* \* cmd 表示每10分钟执行任务


![](img-Linux定时任务\2016-07-08_094405.png)


### 注意事项

在定时任务的最后加上  /dev/null 2>&1(条件：定时任务调用的必须是脚本文件，如果定时任务直接配的是命令不用加)

如果定时任务配的是脚本不用  /dev/null 2>&1会在6.X上/var/spool/mail/root 文件中记录详细的错误日志


## 小结

![](img-Linux定时任务\2016-07-08_164507.png)

![](img-Linux定时任务\2016-07-08_165959.png)

![](img-Linux定时任务\2016-07-08_170348.png)

![](img-Linux定时任务\2016-07-08_171351.png)


[linux定时任务crond那些事](http://oldboy.blog.51cto.com/2561410/1410555 "linux定时任务crond那些事")

[linux定时任务生产java服务无法执行问题群友案例](http://oldboy.blog.51cto.com/2561410/1541515 "linux定时任务生产java服务无法执行问题群友案例")
