# Linux_网络配置

### 网卡查看
```
ifconfig
```

### 配置网卡
```
setup
```
![](img_Linux安装/2016-06-29_223118.jpg)
#### 设备配置
![](img_Linux安装/2016-06-29_223215.jpg)

![](img_Linux安装/2016-06-29_223356.jpg)
#### 设置IP，子网掩码，网关，DNS
![](img_Linux安装/2016-12-15_175255.jpg)
#### 修改eth0网卡信息
```
vim /etc/sysconfig/network-scripts/ifcfg-eth0
```
![](img_Linux安装/2016-12-15_175334.jpg)

![](img_Linux安装/2016-12-15_175213.jpg)

#### 重启网络
```
/etc/init.d/network restart
 service network restart
```


[Linux系统基础网络配置老鸟精华篇](http://oldboy.blog.51cto.com/2561410/784625 "Linux系统基础网络配置老鸟精华篇")
