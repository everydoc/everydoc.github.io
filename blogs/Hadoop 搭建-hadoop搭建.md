---
title: Hadoop 搭建
date: 2021-11-05 23:31:18.384
updated: 2021-11-05 23:31:18.384
url: https://halo.imjcker.com/archives/hadoop搭建
categories: 
tags: bigdata Hadoop
---



基于hadoop-3.2.1，CentOS7，JDK8集群安装配置记录

### 环境配置

#### 域名配置

配置域名映射`vi /etc/hosts`

```shell
172.16.11.129	hadoop-m
172.16.11.130	hadoop-s1
172.16.11.131	hadoop-s2
172.16.11.132	hadoop-s3
```

#### jdk配置

略

#### 环境变量

**为了便于学习，以下操作均在root用户下进行**

解压拷贝hadoop压缩包到`/usr/local/hadoop`

hadoop 环境变量

```shell
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop

export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root

# 刷新配置
source /etc/profile
```



### 配置集群

修改Hadoop、mapreduce、yarn配置的JDK

#### core-site.xml

配置core-site.xml

```xml
<configuration>
 <property>
   <name>fs.defaultFS</name>
   <value>hdfs://hadoop-m:9000</value>
 </property>
 <property>
   <name>hadoop.tmp.dir</name>
   <value>/usr/local/hadoop/data/tmp</value>
 </property>
</configuration>
```

#### hdfs-site.xml

配置hdfs-site.xml

```xml
<property>
  <name>dfs.replication</name>
  <value>3</value>
</property>
<property>
  <name>dfs.name.dir</name>
  <value>/usr/local/hadoop/data/name</value>
</property>
<property>
  <name>dfs.data.dir</name>
  <value>/usr/local/hadoop/data/data</value>
</property>
```

#### mapred-site.xml

```xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
<property>
  <name>mapreduce.application.classpath</name>
  <value>$HADOOP_HOME/share/hadoop/mapreduce/*:$HADOOP_HOME/share/hadoop/mapreduce/lib/*       </value>
</property>
```

#### yarn-site.xml

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
            <name>yarn.resourcemanager.hostname</name>
            <value>hadoop-m</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
      <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME</value>
    </property>
</configuration>
```

#### workers

配置workers

```
hadoop-m
hadoop-s1
hadoop-s2
hadoop-s3
```