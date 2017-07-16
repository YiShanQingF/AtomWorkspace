## 目录
##### [Hadoop 安装 ](http://www.jianshu.com/p/8e57916790f2)
##### [单点启动&集群启动](http://www.jianshu.com/p/715dc8601065)
##### [访问 HDFS](http://www.jianshu.com/p/d8a5459c9f02)
##### [常用配置](http://www.jianshu.com/p/09bd95bdef0f)
##### [常用命令](http://www.jianshu.com/p/2a13831d0e79)


# Hadoop 入门(四)

## 常用配置

### core-site.xml
[core-site.xml](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/core-default.xml)


### hdfs-site.xml
[hdfs-site.xml](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml)

**备份份数**
```
<property>
    <name>dfs.replication</name>
    <value>2</value>
</property>
```
*默认值：3*

**心跳检测时间**
```
<property>
    <name>dfs.namenode.heartbeat.recheck-interval</name>
    <value>20000</value>
</property>
```
*默认值：300000*

**权限检测**
```
<property>
    <name>dfs.permissions.enabled</name>
    <value>false</value>
</property>
```
*默认值：true*



[参考资料 http://mashibing.com/w/](http://mashibing.com/w/)
