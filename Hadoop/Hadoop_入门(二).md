## 目录
##### [Hadoop 安装 ](http://www.jianshu.com/p/8e57916790f2)
##### [单点启动&集群启动](http://www.jianshu.com/p/715dc8601065)
##### [访问 HDFS](http://www.jianshu.com/p/d8a5459c9f02)
##### [常用配置](http://www.jianshu.com/p/09bd95bdef0f)
##### [常用命令](http://www.jianshu.com/p/2a13831d0e79)


# Hadoop 入门(二)

## 单点启动&集群启动
### 单节点启动&停止

**namenode 单节点启动**
```
[root@node0 name]# hadoop-daemon.sh start namenode
starting namenode, logging to /usr/local/hadoop/logs/hadoop-root-namenode-node0.out
[root@node0 name]# jps
3585 Jps
3515 NameNode
```
**datanode 单节点启动**
```
[root@node1 ~]# hadoop-daemon.sh start datanode
starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-node1.out
[root@node1 ~]# jps
2358 DataNode
2391 Jps
```
**namenode 单节点停止**
```
[root@node0 name]# hadoop-daemon.sh stop namenode
stopping namenode
[root@node0 name]# jps
3616 Jps
```
**datanode 单节点停止**
```
[root@node1 ~]# hadoop-daemon.sh stop datanode
stopping datanode
[root@node1 ~]# jps
2465 Jps
```
### 集群启动&停止

**修改配置文件**
```
[root@node0 hadoop]# cd /usr/local/hadoop/etc/hadoop/
[root@node0 hadoop]# vi slaves

```
**/usr/local/hadoop/etc/hadoop/slaves**
```
node1
node2
node3
```
*/usr/local/hadoop/etc/hadoop/slaves 文件中记录 namenode 管理的 datanode*

**整个集群启动**
```
[root@node0 hadoop]# start-dfs.sh
```

**整个集群停止**
```
[root@node0 hadoop]# start-dfs.sh
```
*根据提示输入 node1 node2 node3 的密码即可启动&停止*

**ssh 免密登录**
```
[root@node0 .ssh]# cd /root/.ssh/
[root@node0 .ssh]# ssh-keygen -t rsa
[root@node0 .ssh]# ls
id_rsa  id_rsa.pub  known_hosts

```
*ssh-keygen -t rsa 这个命令执行之后一路回车。*

*完了之后会多两个文件 id_rsa 和 id_rsa.pub。*

*id_rsa 是私钥 id_rsa.pub 是公钥。*

*需要将 id_rsa.pub 公钥复制到 node1 node2 node3 的机器上即可*

```
[root@node0 .ssh]# ssh-copy-id node1

```

**免密启动&停止**
```
[root@node0 .ssh]# start-dfs.sh
Starting namenodes on [node0]
node0: starting namenode, logging to /usr/local/hadoop/logs/hadoop-root-namenode-node0.out
node2: datanode running as process 2516. Stop it first.
node1: starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-node1.out
node3: ssh: connect to host node3 port 22: No route to host
Starting secondary namenodes [0.0.0.0]
0.0.0.0: starting secondarynamenode, logging to /usr/local/hadoop/logs/hadoop-root-secondarynamenode-node0.out
[root@node0 .ssh]# jps
5037 SecondaryNameNode
5149 Jps
4846 NameNode
[root@node0 .ssh]# stop-dfs.sh
Stopping namenodes on [node0]
node0: stopping namenode
node2: stopping datanode
node1: stopping datanode
node3: ssh: connect to host node3 port 22: No route to host
Stopping secondary namenodes [0.0.0.0]
0.0.0.0: stopping secondarynamenode
[root@node0 .ssh]# jps
5464 Jps

```



[参考资料 http://mashibing.com/w/](http://mashibing.com/w/)
