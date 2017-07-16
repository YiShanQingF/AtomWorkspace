# Linux_文件类型和文件扩展名及文件权限权限


**Windows是通过扩展名来区分文件类型**

**Linux文件扩展名和文件类型没有关系**

**为了容易区分和兼容用户使用 Windows 的习惯，我们也会用扩展名来表示 Linux 里的文件类型**


Linux 以前皆文件

### 文件
```
[root@node1 ~]# ll -i
total 92
184799 -rw-------. 1 root root  1721 Jul  1 09:32 anaconda-ks.cfg
186203 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Desktop
186207 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Documents
186204 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Downloads
129294 -rw-r--r--. 1 root root 40745 Jul  1 09:31 install.log
129307 -rw-r--r--. 1 root root  9710 Jul  1 09:30 install.log.syslog
186208 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Music
186209 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Pictures
186206 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Public
186205 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Templates
186210 drwxr-xr-x. 2 root root  4096 Jul  1 09:38 Videos


```
**inode**
包含：文件类型，文件权限，链接数，用户组，大小，时间信息，文件指针

**文件类型**
```
b      block (buffered) special

c      character (unbuffered) special

d      directory

p      named pipe (FIFO)

f      regular file

l      symbolic link; this is never true if the  -L  option  or
        the  -follow  option  is  in effect, unless the symbolic
        link is broken.  If you  want  to  search  for  symbolic
        links when -L is in effect, use -xtype.

s      socket

D      door (Solaris)
```

**文件权限**
![](img-Linux文件类型和文件扩展名及文件权限权限\quanx.png)

**硬链接**
```
[root@node1 ~]# ln install.log shan.log
[root@node1 ~]# ll -i install.log shan.log
129294 -rw-r--r--. 2 root root 40745 Jul  1 09:31 install.log
129294 -rw-r--r--. 2 root root 40745 Jul  1 09:31 shan.log
[root@node1 ~]#
```

硬链接是指通过索引节点(Inode)来进行链接。

在Linux中，多个文件名指向同一个索引节点(Inode)是正常且允许。

**用户**

**用户组**

**大小**

**修改时间**

**文件名**





### 查看文件属性
```
[root@node1 data]# stat shan.txt
  File: `shan.txt'
  Size: 32              Blocks: 8          IO Block: 4096   regular file
Device: 805h/2053d      Inode: 2762        Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2016-07-05 03:51:29.815946744 +0800
Modify: 2016-07-05 03:50:37.659929831 +0800
Change: 2016-07-05 03:50:37.662929853 +0800

```

Access访问时间

Modify修改时间

Change状态改变时间

```
[root@node1 data]# ll -h --time-style=long-iso
total 9.4M
-rw-r--r--. 1 root root 9.3M 2016-07-05 03:21 etc.tar.gz
-rw-r--r--. 1 root root  26K 2016-07-05 03:21 root1.tar.gz
-rw-r--r--. 1 root root  26K 2016-07-05 03:21 root.tar.gz
-rw-r--r--. 1 root root   32 2016-07-05 03:50 shan.txt
drwxr-xr-x. 3 root root 4.0K 2016-07-05 03:29 tmp
drwxr-xr-x. 3 root root 4.0K 2016-07-05 03:31 tmp2

```

## 文件权限

### chmod 改变文件权限属性命令

#### 数字方式修改

```
[root@node1 data]# touch a.sh
[root@node1 data]# ll a.sh
-rw-r--r--. 1 root root 0 Jul  5 17:23 a.sh
[root@node1 data]# chmod 575 a.sh
[root@node1 data]# ll a.sh        
-r-xrwxr-x. 1 root root 0 Jul  5 17:23 a.sh
[root@node1 data]# chmod -R 755  shan
[root@node1 data]# cd shan
[root@node1 shan]# ll
total 0
-rwxr-xr-x. 1 root root 0 Jul  5 17:27 a
-rwxr-xr-x. 1 root root 0 Jul  5 17:27 b
-rwxr-xr-x. 1 root root 0 Jul  5 17:27 c
```
**r**-->4

**w**-->2

**x**-->1

**-**-->0

**-R**-->递归修改

#### 字符方式修改

**chmod [用户类型] [+|-|+] [权限字符] 文件名**

![](img-Linux文件类型和文件扩展名及文件权限权限\2016-07-07_112841.png)

```
[root@node1 data]# chmod u+x shan.txt
[root@node1 data]# ll shan.txt
-rwxr--r--. 1 root root 115 Jul  5 13:42 shan.txt
```

### chown 修改用户所有者和所有组
**chown [选项] ... [所有者][:[组]] 文件...**
```
[root@node1 data]# chown shan shan.txt
[root@node1 data]# chown shan.shan shan.txt
[root@node1 data]# chown root.root shan.txt          
[root@node1 data]# chown .shan shan.txt     


[root@node1 data]# chown -R .shan shan
[root@node1 data]# cd shan
[root@node1 shan]# ll
total 0
-rwxr-xr-x. 1 root shan 0 Jul  5 17:27 a
-rwxr-xr-x. 1 root shan 0 Jul  5 17:27 b
-rwxr-xr-x. 1 root shan 0 Jul  5 17:27 c
[root@node1 shan]#
```


### umask 默认权限分配

```
[root@node1 data]# umask
0022
[qs@node1 ~]$ umask
0002

```

![](img-Linux文件类型和文件扩展名及文件权限权限\2016-07-07_152140.png)



### suid

![](img-Linux文件类型和文件扩展名及文件权限权限\2016-07-07_162649.png)
```
[root@node1 data]# find /usr/bin -type f -perm 4755 -exec ls -lh {} \;
-rwsr-xr-x. 1 root root 35K Oct 15  2014 /usr/bin/newgrp
-rwsr-xr-x. 1 root root 64K Oct 15  2014 /usr/bin/chage
-rwsr-xr-x. 1 root root 49K Jan 30  2012 /usr/bin/at
-rwsr-xr-x. 1 root root 18K Oct 15  2014 /usr/bin/pkexec
-rwsr-xr-x. 1 root root 69K Oct 15  2014 /usr/bin/gpasswd
-rwsr-xr-x. 1 root root 58K Oct 15  2014 /usr/bin/ksu
-rwsr-xr-x. 1 root root 2.3M Oct 18  2014 /usr/bin/Xorg
-rwsr-xr-x. 1 root root 46K Nov 23  2013 /usr/bin/crontab
-rwsr-xr-x. 1 root root 26K Feb 22  2012 /usr/bin/passwd

```


```
[root@node1 data]# ll /var/lib/mlocate/mlocate.db
-rw-r-----. 1 root slocate 2.3M Jul  5 22:49 /var/lib/mlocate/mlocate.db
[root@node1 data]# updatedb
[root@node1 data]# ll `which locate`
-rwx--s--x. 1 root slocate 35K Oct 10  2012 /usr/bin/locate

```

### 粘滞位
```
[root@node1 data]# ll -d /tmp/
drwxrwxrwt. 8 root root 4.0K Jul  5 23:03 /tmp/
[root@node1 tmp]# mkdir data
[root@node1 tmp]# chmod o+t data/
[root@node1 tmp]# ll
total 20K
drwxr-xr-t. 2 root root 4.0K Jul  5 23:15 data
drwx------. 2 shan shan 4.0K Jul  1 09:45 pulse-pirY98xyRuRo
drwx------. 2 root root 4.0K Jul  1 09:44 pulse-tTHcWemH36Mv
drwx------. 2 gdm  gdm  4.0K Jul  1 10:17 pulse-tuxhcH69FODw
-rw-r--r--. 1 root root    7 Jul  5 23:14 root
-rw-------. 1 root root    0 Jul  1 09:17 yum.log
```
一个目录即使他的所有权限都开放 rwxrwxrwx ，如果设置了粘滞位，除非目录的属主和root用户

有权限删除它，其他的用户都不能删除这个目录
