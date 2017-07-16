# ERROR 1364 (HY000): Field 'ssl_cipher' doesn't have a default value


```
[root@qing mysql]# mysql --help |grep Distrib
mysql  Ver 14.14 Distrib 5.6.36, for linux-glibc2.5 (x86_64) using  EditLine wrapper
[root@qing mysql]# mysql -V
mysql  Ver 14.14 Distrib 5.6.36, for linux-glibc2.5 (x86_64) using  EditLine wrapper
[root@qing mysql]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.6.36 MySQL Community Server (GPL)

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select version();
+-----------+
| version() |
+-----------+
| 5.6.36    |
+-----------+
1 row in set (0.00 sec)

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.36, for linux-glibc2.5 (x86_64) using  EditLine wrapper

Connection id:          4
Current database:
Current user:           root@localhost
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.36 MySQL Community Server (GPL)
Protocol version:       10
Connection:             Localhost via UNIX socket
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
UNIX socket:            /tmp/mysql.sock
Uptime:                 4 min 50 sec

Threads: 2  Questions: 8  Slow queries: 0  Opens: 67  Flush tables: 1  Open tables: 60  Queries per second avg: 0.027
--------------

mysql>

```


```
mysql> insert into user(host,user,password) values("localhost","peter1",password("123456"));
ERROR 1364 (HY000): Field 'ssl_cipher' doesn't have a default value



原因：在我的配置文件my.cnf中有这样一条语句

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

指定了严格模式，为了安全，严格模式禁止通过insert 这种形式直接修改mysql库中的user表进行添加新用户



解决办法：

将配置文件中的STRICT_TRANS_TABLES删掉，即改为：

sql_mode=NO_ENGINE_SUBSTITUTION

然后重启mysql即可

```


[参考资料 http://blog.csdn.net/u012377333/article/details/50290827](http://blog.csdn.net/u012377333/article/details/50290827)
