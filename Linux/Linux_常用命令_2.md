
# Linux 常用命令

## 系统信息查看

### 查看系统内核版本 `cat /etc/issue`

```
[root@redis ~]# cat /etc/issue
CentOS release 6.5 (Final)
Kernel \r on an \m
显示了系统名称(CentOS)和内核版本（release 6.5）
```

### 系统信息 `uname -a`
```
[root@redis ~]# uname -a
Linux redis 2.6.32-431.el6.x86_64 #1 SMP Fri Nov 22 03:15:09 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux
显示系统名、节点名称、操作系统的发行版号、操作系统版本、运行系统的机器 ID 号
```

### 内存信息查看 `free -m`
```
[root@redis ~]# free -m
             total       used       free     shared    buffers     cached
Mem:           980        912         68          0         21        719
-/+ buffers/cache:        171        809
Swap:         2047          0       2047
```

```
第2行:
otal 内存总数: 980【注意单位是M】
used 已经使用的内存数: 912
free 空闲的内存数: 68
shared 当前已经废弃不用，总是0
buffers: Buffer Cache内存数: 21
cached: Page Cache内存数: 719
关系：total = used + free
第3行:
-/+ buffers/cache的意思:
-buffers/cache 的内存数: 171 (等于第1行的 used - buffers - cached)
+buffers/cache 的内存数: 809 (等于第1行的 free + buffers + cached)
注:此处的内存数在用上面式子计算后，在大小上有一点点出入(还不知道是什么原因)。
可见-buffers/cache反映的是被程序实实在在吃掉的内存，而+buffers/cache反映的是可以挪用的内存总数。
```

### 系统负载查看 `uptime` `top`

**uptime**
```
[root@redis ~]# uptime
 20:49:33 up  4:28,  4 users,  load average: 0.00, 0.00, 0.00
```
```
当前时间 20:49:33
系统已运行的时间 4:28
当前在线用户 4 user
平均负载：0.00, 0.00, 0.00，最近1分钟、5分钟、15分钟系统的负载
```
**top**

[参考资料](http://jingyan.baidu.com/album/4d58d5412917cb9dd4e9c0ed.html?picindex=2)

### 进程 `ps -ef|grep tty`
```
[root@redis ~]# ps -ef|grep tty
root       1394      1  0 16:21 tty2     00:00:00 /sbin/mingetty /dev/tty2
root       1396      1  0 16:21 tty3     00:00:00 /sbin/mingetty /dev/tty3
root       1398      1  0 16:21 tty4     00:00:00 /sbin/mingetty /dev/tty4
root       1400      1  0 16:21 tty5     00:00:00 /sbin/mingetty /dev/tty5
root       1402      1  0 16:21 tty6     00:00:00 /sbin/mingetty /dev/tty6
root      20310  20237  0 20:09 tty1     00:00:00 -bash
root      20413  20330  0 20:55 pts/2    00:00:00 grep tty
```

### 端口占用
```
[root@redis redis-3.0.4]# netstat -nltup
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name   
tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      1260/sshd           
tcp        0      0 127.0.0.1:25                0.0.0.0:*                   LISTEN      1336/master         
tcp        0      0 :::22                       :::*                        LISTEN      1260/sshd           
tcp        0      0 ::1:25                      :::*                        LISTEN      1336/master
```
```
参数：
-a (all)显示所有选项，默认不显示LISTEN相关
-t (tcp)仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化成数字。
-l 仅列出有在 Listen (监听) 的服務状态
-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命令。
提示：LISTEN和LISTENING的状态只有用-a或者-l才能看到
```

## 磁盘信息查看

### 系统磁盘空间查看 `df -h`
```
[root@redis ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        18G  2.9G   14G  18% /
tmpfs           491M     0  491M   0% /dev/shm
/dev/sda1       194M   29M  155M  16% /boot
```

### 单个文件夹大小查看 `du -h --max-depth=1 /opt`
```
[root@redis ~]# du -h --max-depth=1 /opt
46M     /opt/redis-3.0.4
4.0K    /opt/rh
47M     /opt
```

## FTP















































[参考资料](https://mp.weixin.qq.com/s?__biz=MzA3OTgyMDcwNg==&mid=2650630650&idx=1&sn=5ee738df67616e449e7ccb2f6ea947fc&chksm=87a46b37b0d3e2217bf5c08f689f41f2e15d7811ec03467d913f139cb600658015dadb826457&mpshare=1&scene=24&srcid=0706VGEb1p9i6BN0LkxS7mrn#rd)
