---
title: Kafka All in One
date: 2021-11-05 23:32:18.619
updated: 2022-01-19 01:10:15.166
url: https://halo.imjcker.com/archives/kafka集群
categories: 
tags: kafka | zookeeper | MQ
---


![kafka](https://imjcker.com:1990/upload/2022/01/kafka-a26e91c4fadb47e690e3015bc7e26e79.png)
## zookeeper

zookeeper 集群搭建记录，你懂的！！！

### docker 安装

#### 单节点安装

```shell
docker run -d --name zookeeper-server-imjcker -p 2181:2181 -p 2888:2888 -p 3888:3888 -d --restart always zookeeper
# 查看日志
docker logs zookeeper-server-imjcker
# 进入容器
docker exec -it zookeeper-server-imjcker /bin/bash
```

#### 集群配置

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

### 手动安装部署

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

### 其它配置

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

### 开机启动

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



## Kafka





如果你看到了这片文章，说明你知道了kafka是基于zookeeper的，本文主要是在上文的基础上搭建Kafka集群。

1. 下载

打开<http://kafka.apache.org/downloads.html>

选择指定版本

```shell
wget https://downloads.apache.org/kafka/2.5.0/kafka_2.13-2.5.0.tgz

tar zxf kafka_2.13-2.5.0.tgz -C /usr/local/

mv /usr/local/kafka_2.13-2.5.0 /usr/local/kafka

```

2. 修改配置文件

vi /usr/local/kafka/config/server.properties

```properties
#broker.id=0  每台服务器的broker.id都不能相同
broker.id=1
#hostname
host.name=172.16.11.130

#在log.retention.hours=168 下面新增下面三项
message.max.byte=5242880
default.replication.factor=2
replica.fetch.max.bytes=5242880

#设置zookeeper的连接端口
zookeeper.connect=172.16.11.130:2181,172.16.11.131:2181,172.16.11.132:2181

```

3. 配置kafka环境变量

```shell
# vim /etc/profile
echo "export KAFKA_HOME=/usr/local/kafka" >> /etc/profile
echo "export PATH=$PATH:$KAFKA_HOME/bin" >> /etc/profile

# source /etc/profile

```

4. 启动

```shell
/usr/local/kafka/bin/kafka-server-start.sh -daemon /usr/local/kafka/config/server.properties

```

5. 验证

```shell
jps 打印出kafka的进程

```

6. 创建topic

```shell
#创建Topic
kafka-topics.sh --create --zookeeper zk1:2181 --partitions 3 --replication-factor 3 --topic imjcker-kafka-test-topic
#解释
--partitions 3   #创建3个分区
--replication-factor 3     #复制3份
--topic     #主题为imjcker-kafka-test-topic

#查看topic状态
kafka-topics.sh --describe --zookeeper localhost:2181 --topic imjcker-kafka-test-topic

#下面是显示信息
Topic:imjcker   PartitionCount:3    ReplicationFactor:3 Configs:
Topic: imjcker   Partition: 0    Leader: 1   Replicas: 1,2,3 Isr: 1,2,3
Topic: imjcker 	 Partition: 1    Leader: 2   Replicas: 2,3,1 Isr: 2,3,1
Topic: imjcker   Partition: 2    Leader: 3   Replicas: 3,1,2 Isr: 3,1,2

状态说明：
#imjcker有三个分区分别为1、2、3;
#分区0的leader是1（broker.id），分区0有三个副本，并且状态都为lsr（ln-sync，表示可以参加选举成为leader）。

#删除topic
    在config/server.properties中加入delete.topic.enable=true并重启服务，在执行如下命令
# kafka-topics.sh --delete --zookeeper localhost:2181 --topic imjcker

```

7. 测试消息

```shell
#在一台服务器上创建一个发布者-发送消息
kafka-console-producer.sh --broker-list 172.32.15.114:9092 --topic imjcker
输入以下信息：
　　This is a message
　　This is another message

#在另一台服务器上创建一个订阅者接收消息
kafka-console-consumer.sh --zookeeper 172.32.15.112:2181 --topic imjcker --from-beginning

#--from-beginning 表示从开始第一个消息开始接收
#测试（订阅者那里能正常收到发布者发布的消息，则说明已经搭建成功）

```

8. Kafka Manager

   docker 

   ```shell
   docker run -d --name kafka-manager --restart always -p 9000:9000 --network host -e ZK_HOSTS="localhost:2181" -e APPLICATION_SECRET=kafka-manager sheepkiller/kafka-manager
   ```



### 附录

#### 消息丢失问题

https://www.jianshu.com/p/4491cba335d1