# swap 分区未启用


## 查看 swap 分区信息
```
[root@oracle ~]# free -m
             total       used       free     shared    buffers     cached
Mem:          1861       1144        716          0         74        874
-/+ buffers/cache:        196       1665
Swap:         2047          0       2047
[root@oracle ~]# swapon -s
Filename                                Type            Size    Used    Priority
/dev/sda2                               partition       2097144 0       -1
[root@oracle ~]# cat /proc/swaps
Filename                                Type            Size    Used    Priority
/dev/sda2                               partition       2097144 0       -1

```


## 临时启用 swap 分区
### 检查磁盘是否给 swap 分区
```
[root@oracle ~]# fdisk -l

Disk /dev/sda: 32.2 GB, 32212254720 bytes
255 heads, 63 sectors/track, 3916 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000030a6

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1          26      204800   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sda2              26         287     2097152   82  Linux swap / Solaris
Partition 2 does not end on cylinder boundary.
/dev/sda3             287        3917    29154304   83  Linux
```
用 `fdisk` 命令查看系统分区信息

### 格式化 swap 分区
```
mkswap /dev/sda2
```

### 挂载 swap 分区
```
swapon /dev/sda2
```

## 设置开机启用 swap 分区
### 查看 /etc/fstab
```
[root@oracle ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Fri Dec 16 01:15:59 2016
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=e447cbd9-3cbb-45dd-96a2-b2a430499e04 /                       ext4    defaults        1 1
UUID=bdcacf69-ab4e-450a-ab95-e4ab5734a5e1 /boot                   ext4    defaults        1 2
UUID=7912c75d-6939-478e-bfb8-2a6edca46bb7 swap                    swap    defaults        0 0
tmpfs                   /dev/shm                tmpfs   defaults        0 0
devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
sysfs                   /sys                    sysfs   defaults        0 0
proc                    /proc                   proc    defaults        0 0
```

可以看到第三个 UUID 就是 swap 分区启用的信息

如果重启 swap 分区未启用一般是注释或者删除了第三行

我们要做的就是将他修改正确即可


### 查询 UUID 分区ID
```
[root@oracle ~]# blkid -t TYPE=swap
/dev/sda2: UUID="7912c75d-6939-478e-bfb8-2a6edca46bb7" TYPE="swap"
[root@oracle ~]# blkid -t TYPE=ext4
/dev/sda3: UUID="e447cbd9-3cbb-45dd-96a2-b2a430499e04" TYPE="ext4"
/dev/sda1: UUID="bdcacf69-ab4e-450a-ab95-e4ab5734a5e1" TYPE="ext4"
```

### 使修改生效
```
swapon -av
```

**参考资料**

[一个问题引发对Linux swap和内存的思考 ](http://blog.chinaunix.net/uid-20786165-id-3172090.html)

[Linux命令-自动挂载文件/etc/fstab功能详解](http://www.cnblogs.com/qiyebao/p/4484047.html)

[/etc/fstab 文件解释](http://ckc620.blog.51cto.com/631254/394238/)
