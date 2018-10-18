### 初识 RabbitMQ

RabbitMQ 是一个开源的消息代理和队列服务器，用来通过普通协议在完全不同的应用之间共享数据，RabbitMQ 是使用 Erlang 语言来编写的，并且 RabbitMQ 是基于 AMQP 协议的。

### 安装

```
http://www.rabbitmq.com/
```

1 准备：yum install build-essential openssl openssl-devel unixODBC unixODBC-devel make gcc gcc-c++ kernel-devel m4 ncurses-devel tk tc xz

2 下载：wget www.rabbitmq.com/releases/erlang/erlang-18.3-1.el7.centos.x86_64.rpm
wget http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm
wget www.rabbitmq.com/releases/rabbitmq-server/v3.6.5/rabbitmq-server-3.6.5-1.noarch.rpm

3配置 vim /etc/hosts 以及 /etc/hostname  (Linux防火墙)

3 配置文件：vim /usr/lib/rabbitmq/lib/rabbitmq_server-3.6.5/ebin/rabbit.app

比如修改密码、配置等等，例如：loopback_users 中的 <<"guest">>,只保留guest
服务启动和停止：
启动 rabbitmq-server start &
停止 rabbitmqctl app_stop

4 管理插件：rabbitmq-plugins enable rabbitmq_management
5 访问地址：http://192.168.11.81:15672/

Git 忽略一些文件不加入版本控制
https://www.jianshu.com/p/1a0e282026a5