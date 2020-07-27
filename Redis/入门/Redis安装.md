

## Redis

### 下载地址

[下载地址](http://download.redis.io/releases/ "download.redis.io")

### 下载、解压、编译Redis
```
wget http://download.redis.io/releases/redis-5.0.0.tar.gz
tar xzf redis-5.0.0.tar.gz
cd redis-5.0.0
make

make PREFIX=/usr/local/redis install            制定路径安装

cp /root/redis-5.0.2/redis.conf /usr/local/redis/redis.conf         复制配置文件

vi redis.conf       修改配置文件
daemonize no 修改为 daemonize yes
/usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
https://www.cnblogs.com/xiangzideheiniu/p/12430364.html
```

### 基本文件介绍

```


-rwxr-xr-x. 1 root root 4.2M 11月 25 07:15 redis-benchmark       性能测试
-rwxr-xr-x. 1 root root 7.8M 11月 25 07:15 redis-check-rdb       aof文件修复工具
-rwxr-xr-x. 1 root root 4.6M 11月 25 07:15 redis-cli             命令行客户端
lrwxrwxrwx. 1 root root   12 11月 25 07:15 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 7.8M 11月 25 07:15 redis-server          Redis启动命令

```


### 数据类型

#### 字符串(String)
#### 哈希
#### 字符串列表(String)
#### 字符串集合(String)
#### 有序字符串集合(String)
