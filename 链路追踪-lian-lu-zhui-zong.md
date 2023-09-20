---
title: 链路追踪
date: 2022-01-18 18:36:05.253
updated: 2022-01-18 18:36:05.253
url: https://halo.imjcker.com/archives/lian-lu-zhui-zong
categories: 默认分类
tags: pinpoint APM skywalking
---

![pinpoint](https://imjcker.com:1990/upload/2022/01/pinpoint-7d867719a33f44ce92c3278052b29a08.png)

搭建pinpoint环境和使用配置记录📝



## pinpoint



## 环境

CentOS 7、Java 8、HBase1.3、pinpoint1.8.5



## HBase搭建

### 默认配置

如果默认配置启动失败。

解决办法：配置/etc/hosts

配置127.0.1.1 对应到hostname 如：

```shell
127.0.1.1 imjcker.com
```

启动hbase

```shell
./bin/start-hbase.sh
```

检查启动状态

```shell
jps -l
```



### 配置zookeeper

#### 默认情况

默认情况下hbase使用自带的zookeeper，所以无需任何配置，如果有高可用的zookeeper集群，推荐使用外部配置的zookeeper集群。

#### 配置zookeeper

1. 配置hbase-env.sh

   aaaa

2. 配置hbase-site.xml

   bbb

3. 配置regionservers

   ccc

启动报错：

```shell
Could not start ZK with 3 ZK servers in local mode deployment. Aborting as clients (e.g. shell) will not be able to find this ZK quorum.
```

原因：

未启动hbase集群模式 修改hbase-site.xml启动集群配置



## 配置pinpoint-collector



## 配置pinpoint-web

### 配置mysql

在tomcat下配置



## 配置pinpoint-agent



## skywalking

学习使用APM系统，skywalking-6.5搭建和使用记录



