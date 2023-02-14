---
title: zookeeper集群
date: 2021-11-05 23:32:20.925
updated: 2021-11-05 23:32:20.925
url: https://halo.imjcker.com/archives/zookeeper集群
categories: 
tags: zookeeper
---


zookeeper 集群搭建记录，你懂的！！！

## docker 安装

### 单节点安装

```shell
docker run -d --name zookeeper-server-imjcker -p 2181:2181 -p 2888:2888 -p 3888:3888 -d --restart always zookeeper
# 查看日志
docker logs zookeeper-server-imjcker
# 进入容器
docker exec -it zookeeper-server-imjcker /bin/bash
```

### 集群配置

```yaml
version: '3.1'

services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zoo1
    ports:
      - 2181:2181
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=0.0.0.0:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo2:
    image: zookeeper
    restart: always
    hostname: zoo2
    ports:
      - 2182:2181
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=0.0.0.0:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo3:
    image: zookeeper
    restart: always
    hostname: zoo3
    ports:
      - 2183:2181
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=0.0.0.0:2888:3888;2181
```



## 手动安装部署

环境，Java安装

准备目录

```shell
mkdir /usr/local/zookeeper/data -p
mkdir /usr/local/zookeeper/log
```

```shell
# 下载安装包
wget https://downloads.apache.org/zookeeper/zookeeper-3.6.0/apache-zookeeper-3.6.0-bin.tar.gz
# 解压到指定路径
tar zxf apache-zookeeper-3.6.0-bin.tar.gz -C /usr/local
# 修改配置文件
cd /usr/local/zookeeper/conf
# 拷贝配置文件
cp zoo_sample.cfg zoo.cfg
```



编写配置文件内容

```shell
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/zookeeper/data
dataLogDir=/usr/local/zookeeper/log
clientPort=2181
server.1=172.16.11.130:2888:3888
server.2=172.16.11.131:2888:3888
server.3=172.16.11.132:2888:3888
```

配置myid文件

```shell
#server1
echo "1" > /usr/local/zookeeper/data/myid
#server2
echo "2" > /usr/local/zookeeper/data/myid
#server3
echo "3" > /usr/local/zookeeper/data/myid
```

配置环境变量

```shell
vi /etc/profile

export ZOOKEEPER_HOME=/usr/local/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin

source /etc/profile
```



```shell
# 启动
/usr/local/zookeeper/bin/zkServer.sh start
# 查看
/usr/local/zookeeper/bin/zkServer.sh status
```





## 其它配置

启动失败原因：

1. 可能是虚拟机内存分配太少了
2. 检查配置
3. 如果启动成功，但是集群状态失败，请关闭防火墙后再检查状态

***为什么要关闭防火墙？*** 不用开启连接端口和集群通信端口即可，不必关闭防火墙。

不知道，只知道在配置zookeeper集群是启动失败，根据报错到Google查询得知要关闭防火墙。

```shell
firewall-cmd --add-port=2181/tcp --zone=public --permanent
firewall-cmd --add-port=2888/tcp --zone=public --permanent
firewall-cmd --add-port=3888/tcp --zone=public --permanent
firewall-cmd --add-port=12181/tcp --zone=public --permanent
```



## 开机启动

```shell
[Unit]
Description=zookeeper Server Daemon

[Service]
Type=simple
ExecStartPre=-/usr/sbin/setcap cap_net_bind_service=+ep /usr/local/zookeeper/bin/zkServer.sh
ExecStart=/usr/local/zookeeper/bin/zkServer.sh start
Restart=always
RestartSec=60s
User=nobody
PermissionsStartOnly=true

[Install]
WantedBy=multi-user.target
```