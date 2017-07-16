# Linux_正则表达式


```
以行为单位对字符串进行处理

```



**^**以某某开头

**$**以某某结尾

**.**任意字符

**\**转译字符

**\***重复0个或多个前面的一个字符

**[]**字符集合中的任意字符的符号

**[^work ]**匹配不包含^后的任意字符

**a\\{n,m\\}**a重复n到m次

**a\\{n,\\}**a重复最少n次

**a\\{n\\}**a重复n次

```
[root@node1 data]# grep "490\{3,5\}448" shan.txt
49000448
490000448
[root@node1 data]# grep "490\{2,5\}448" shan.txt  
49000448
490000448
4900448
[root@node1 data]# grep "490\{3,\}448" shan.txt   
49000448
490000448
[root@node1 data]# grep "490\{3\}448" shan.txt   
49000448
[root@node1 data]#

```

**+**重复一个或一个以上前面的字符
```
[root@node1 data]# grep "490+448" shan.txt
[root@node1 data]# egrep "490+448" shan.txt
49000448
490000448
490448
4900448
[root@node1 data]# grep -E "490+448" shan.txt    
49000448
490000448
490448
4900448

```

**?**重复0个或一个
```
[root@node1 data]# grep "490?448" shan.txt    
[root@node1 data]# grep -E "490?448" shan.txt
49448
490448
[root@node1 data]# egrep "490?448" shan.txt    
49448
490448

```

**|**用户或的方式查找多个符合的字符串
```
[root@node1 data]#  egrep "3306|1521" /etc/services
mysql           3306/tcp                        # MySQL
mysql           3306/udp                        # MySQL
ncube-lm        1521/tcp                # nCube License Manager
ncube-lm        1521/udp                # nCube License Manager

```

**()**找出“用户组”字符串
```
[root@node1 data]# grep -E "s(ha|a)n" shan.txt     
i am qingshan,myqq is 123456789
shan
san
[root@node1 data]# grep -E "s(ha|a|h)n" shan.txt
i am qingshan,myqq is 123456789
shan
shn
san

```


### 取出eth0的IP地址
```
[root@node1 data]# ifconfig eth0
eth0      Link encap:Ethernet  HWaddr 00:0C:29:03:02:17  
          inet addr:192.168.75.128  Bcast:192.168.75.255  Mask:255.255.255.0
          inet6 addr: fe80::20c:29ff:fe03:217/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:26151 errors:0 dropped:0 overruns:0 frame:0
          TX packets:55259 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2718314 (2.5 MiB)  TX bytes:54945993 (52.4 MiB)
          Interrupt:19 Base address:0x2000

[root@node1 data]# ifconfig eth0|grep "inet addr"
          inet addr:192.168.75.128  Bcast:192.168.75.255  Mask:255.255.255.0
[root@node1 data]# ifconfig eth0|grep "inet addr"|cut -d ":" -f2|cut -d " " -f1
192.168.75.128


[root@node1 data]# ifconfig eth0|grep "inet addr"|awk -F ":" '{print $2}'
192.168.75.128  Bcast
[root@node1 data]# ifconfig eth0|grep "inet addr"|awk -F ":" '{print $2}'|awk -F" " '{print $1}'
192.168.75.128


[root@node1 data]# ifconfig eth0|grep "inet addr"|awk -F "[: ]" '{print $13}'
192.168.75.128
[root@node1 data]# ifconfig eth0|grep "inet addr"|awk -F "[: ]+" '{print $4}'
192.168.75.128


[root@node1 data]# ifconfig eth0|sed -n '2p'
          inet addr:192.168.75.128  Bcast:192.168.75.255  Mask:255.255.255.0
[root@node1 data]# ifconfig eth0|sed -n '2p'|awk -F "[: ]+" '{print $4}'
192.168.75.128


[root@node1 data]# ifconfig eth0|sed -n '2p'|awk -F "[: ]+" 'NR=2{print $4}'
192.168.75.128

[root@node0 ~]# ifconfig eth0|sed -n '/inet addr/p'|sed 's#^.*addr:##g'
192.168.1.86  Bcast:192.168.1.255  Mask:255.255.255.0
[root@node0 ~]# ifconfig eth0|sed -n '/inet addr/p'|sed 's#^.*addr:##g'|sed 's#  Bc.*$##g'
192.168.1.86


[root@node0 ~]# ifconfig eth0|sed -n 's#^.*addr:\(.*\)  Bcast:.*$#\1#gp'
192.168.1.86


[root@node0 ~]# ifconfig eth0|sed -n 's#^.*dr:\([0-9].*\)  Bcast:\([0-9].*\)  Ma.*$#\1 \2#gp'
192.168.1.86 192.168.1.255

```

![](img-Linux正则表达式\2016-07-06_212824.jpg)


### 排除空行

```
[root@node0 data]# grep -v "^$" shan.txt
abcd
ef
g
12345
67890
[root@node0 data]# sed '/^$/d' shan.txt
abcd
ef
g
12345
67890
[root@node0 data]# awk /^[^$]/ shan.txt
abcd
ef
g
12345
67890

```
