# Linux_用户用户组

### groupadd 添加组

### groupdel 删除加组

### useradd 添加用户
在添加用户时没有指定用户组时，系统会自动创建一个与用户名同名的用户组

![](img-Linux用户用户组\2016-07-11_093742.png)

![](img-Linux用户用户组\2016-07-08_174938.png)

![](img-Linux用户用户组\2016-07-08_174959.png)

### passwd  --stdin 修改用户密码，如果不知道用户，则修改当前用户密码，普通用户只能修改自己的密码

|参数	|说明	|
| --:	|:--	|
|--stdin|非交互式修改，可用于批量修改				 |
|-l     |锁定用户，不能更改密码(root可以修改)		 |
|-u     |解锁		 |
|-S     |查看状态	 |


```
[root@node1 ~]# passwd --stdin shan
Changing password for user shan.
1qaz2wsx
passwd: all authentication tokens updated successfully.
[root@node1 ~]# passwd qs
Changing password for user qs.
New password:
BAD PASSWORD: it is based on a dictionary word
Retype new password:
passwd: all authentication tokens updated successfully.
```

### chage 修改用户密码有效期

```
[root@node1 ~]# chage -l qs
Last password change                                    : Jul 05, 2016 <=== -d
Password expires                                        : never        <===		
Password inactive                                       : never        <===	-I
Account expires                                         : never        <===	-E
Minimum number of days between password change          : 0            <===	-m
Maximum number of days between password change          : 99999        <===	-M
Number of days of warning before password expires       : 7            <===	-W
```

**7天内不能改密码，60天后必须改密码，过期前10天通知用户改密码，过期30天后禁止用户登录**

```
[root@node1 ~]# chage -m 7 -M 60 -W 10 -I 30 qs
[root@node1 ~]# chage -l qs
Last password change                                    : Jul 05, 2016
Password expires                                        : Sep 03, 2016
Password inactive                                       : Oct 03, 2016
Account expires                                         : never
Minimum number of days between password change          : 7
Maximum number of days between password change          : 60
Number of days of warning before password expires       : 10

```

![](img-Linux用户用户组\2016-07-11_105532.png)

### userdel 删除用户

|参数	|说明	|
| --:	|:--	|
|-r     |删除家目录				 |
|-l     |锁定用户，不能更改密码(root可以修改)		 |
|-u     |解锁		 |
|-S     |查看状态	 |

### usermod 修改用户的命令，用来修改登录名，用户的家目录

![](img-Linux用户用户组\2016-07-11_111501.png)
![](img-Linux用户用户组\2016-07-11_111537.png)
![](img-Linux用户用户组\2016-07-11_111614.png)

### id 查看用户的UID GID 及所属用户组

### su 切换用户

### sudo

```
/tmp/etctar/
``` 

![](img-Linux用户用户组\2016-07-11_145410.png)

![](img-Linux用户用户组\2016-07-11_113957.png)

### visudo

### whoami 查看当前命令行的用户

### 重要文件

**/etc/passwd**用户

|root	 |:x        |:0   |:0   |:root      |:/root      |:/bin/bash	|
|:--:    |:--:      |:--: |:--: |:--:       |:--:        |:--:			|
|用户名称|:用户密码	|:UID |:GID	|:用户说明	|:用户家目录 |:shell解释器	|

**/etc/shadow**用户密码

**/etc/group**用户组

**/etc/gshadow**用户组密码

锁定文件
```
[root@node1 /]# chattr +i /etc/passwd /etc/shadow /etc/group /etc/gshadow
[root@node1 /]# chattr -i /etc/passwd /etc/shadow /etc/group /etc/gshadow

```


### UID
**0**超级用户

**1-499**虚拟用户

**500-65535**普通用户


### 用户和用户组

![](img-Linux用户用户组\2016-07-06_090848.png)


### useradd不需要密码的用户
服务的运行时需要用户角色，可以不用登陆，因此工作中我们需要运行 MySQL 数据库

**可以创建如下用户**
```
[root@node1 /]# groupadd mysql -g 49
[root@node1 /]# useradd mysql -u 49 -s /sbin/nologin -g mysql
[root@node1 /]# id mysql
uid=49(mysql) gid=49(mysql) groups=49(mysql)

```

### 查看用户ID
```
[root@node1 /]# id root
uid=0(root) gid=0(root) groups=0(root)
```

### 系统所有用户
```
[root@node1 /]# awk -F ":" '{print $1}' /etc/passwd
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
uucp
operator
games
gopher
ftp
nobody
dbus
rpc
vcsa
rtkit
avahi-autoipd
pulse
haldaemon
ntp
saslauth
postfix
abrt
rpcuser
nfsnobody
gdm
sshd
tcpdump
oprofile
shan
qs

```

### 文件、服务、进程所属关系

用户诞生属于一个组

文件诞生属于用户

服务或者进程运行对应一个用户


### /etc/skel

用来存放新用户配置文件的目录，添加新用户时，这个目录下的所有文件会自动复制到新用户的家目录下；

默认情况， /etc/skel 目录下的所有文件都是隐藏文件(以.开头)

通过修改、添加、删除 /etc/skel 目录下的文件，可以为新创建的用户提供统一的初始化环境

```
[root@node1 ~]# ll /etc/skel/ -ah
total 36K
drwxr-xr-x.   4 root root 4.0K Jul  1 09:26 .
drwxr-xr-x. 102 root root  12K Jul 11 00:05 ..
-rw-r--r--.   1 root root   18 Oct 16  2014 .bash_logout
-rw-r--r--.   1 root root  176 Oct 16  2014 .bash_profile
-rw-r--r--.   1 root root  124 Oct 16  2014 .bashrc
drwxr-xr-x.   2 root root 4.0K Nov 12  2010 .gnome2
drwxr-xr-x.   4 root root 4.0K Jul  1 09:22 .mozilla

```

### /etc/login.defs

**配置创建新用户属和密码相关的文件**

```
[root@node1 keyrings]# cat  /etc/login.defs
#
# Please note that the parameters in this configuration file control the
# behavior of the tools from the shadow-utils component. None of these
# tools uses the PAM mechanism, and the utilities that use PAM (such as the
# passwd command) should therefore be configured elsewhere. Refer to
# /etc/pam.d/system-auth for more information.
#

# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR      Maildir
MAIL_DIR        /var/spool/mail
#MAIL_FILE      .mail

# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_MIN_LEN    Minimum acceptable password length.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_MIN_LEN    5
PASS_WARN_AGE   7

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                   500
UID_MAX                 60000

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                   500
GID_MAX                 60000

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD    /usr/sbin/userdel_local

#
# If useradd should create home directories for users by default
# On RH systems, we do. This option is overridden with the -m flag on
# useradd command line.
#
CREATE_HOME     yes

# The permission mask is initialized to this value. If not specified, 
# the permission mask will be initialized to 022.
UMASK           077

# This enables userdel to remove user groups if no members exist.
#
USERGROUPS_ENAB yes

# Use SHA512 to encrypt password.
ENCRYPT_METHOD SHA512 

```


### /etc/default/useradd

```
# useradd defaults file
GROUP=100
HOME=/home
INACTIVE=-1		是否启用账号过期停权，-1 表示不启用
EXPIRE=		过期时间
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```


### 日志审计
```
[root@node1 etctar]# rpm -qa|egrep "sudo|rsyslog"
rsyslog-5.8.10-8.el6.i686
sudo-1.8.6p3-15.el6.i686
[root@node1 etctar]# tail -1 /etc/sudoser
tail: cannot open `/etc/sudoser' for reading: No such file or directory
[root@node1 etctar]# echo "Defaults      logfile=/var/log/sudo.log">>/etc/sudoser
[root@node1 etctar]# tail -1 /etc/sudoser                                    Defaults      logfile=/var/log/sudo.log
[root@node1 etctar]# visudo -c
/etc/sudoers: parsed OK
[root@node1 etctar]# echo "local2.debug    /var/log/sudo.log">>/etc/rsyslog.conf 
[root@node1 etctar]# /etc/init.d/rsyslog restart
Shutting down system logger:                               [  OK  ]
Starting system logger:                                    [  OK  ]
[root@node1 etctar]# ll /var/log/sudo.log 
-rw-------. 1 root root 0 Jul 11 07:33 /var/log/sudo.log



```


























