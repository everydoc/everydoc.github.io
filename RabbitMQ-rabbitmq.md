---
title: RabbitMQ
date: 2021-11-05 23:32:20.844
updated: 2021-11-05 23:32:20.844
url: https://halo.imjcker.com/archives/rabbitmq
categories: 
tags: RabbitMQ
---


RabbitMQ是开源市场上的一个佼佼者，被众多的大小企业使用。本文主要记录了RabbitMQ的搭建工作。

## 基于docker 安装

启动docker中的centso 7容器，模拟真实服务器安装。

```shell
sudo docker run --privileged --cap-add SYS_ADMIN -e container=docker -it \
--name rabbit-1 \
--hostname rabbit-1 \
-p 5672:5672 -p 15672:15672 -p 25672:25672 \
-d  --restart=always \
centos:7 /usr/sbin/init
```

进入centos 7系统

```shell
docker exec -it rabbit-1 /bin/bash
```

下载rabbitmq安装包

```shell
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.0/rabbitmq-server-3.8.0-1.el7.noarch.rpm
```

下载erlang安转包

```shell
wget https://github.com/rabbitmq/erlang-rpm/releases/download/v22.1/erlang-22.1-1.el6.x86_64.rpm
```

安装erlang

```shell
yum install erlang-22.1-1.el6.x86_64.rpm
```

安装rabbitmq

```shell
yum install rabbitmq-server-3.8.0-1.el7.noarch.rpm
```

开启开机启动rabbitmq服务

```shell
systemctl enable rabbitmq-server
```

启动rabbitmq服务

```shell
systemctl start rabbitmq-server
```

拉去docker镜像

```shell
docker pull rabbitmq

docker run -d --name rabbitmq -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=rabbitmq rabbitmq:3-management
```

**添加用户开启远程**

* 进入rabbitmq服务

  ```shell
  docker exec -it rabbit-server-1 /bin/bash
  ```

- 创建账号

```bash
rabbitmqctl add_user admin rabbitmq
```

- 设置用户角色

```bash
rabbitmqctl  set_user_tags  admin  administrator
```

- 设置用户权限

```bash
rabbitmqctl set_permissions -p "/" admin ".*" ".*" ".*"
```

- 设置完成后可以查看当前用户和角色(需要开启服务)

```undefined
rabbitmqctl list_users
```

这是你就可以通过其他主机的访问RabbitMQ的Web管理界面了，访问方式，浏览器输入：`serverip:15672`。其中serverip是RabbitMQ-Server所在主机的ip。

## 集群

**重启无法加入集群**

删除安装目录下的`var/lib/rabbitmq/mnesia`目录重启即可