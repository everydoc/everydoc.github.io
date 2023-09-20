---
title: JVM All In One
date: 2022-01-19 01:06:26.565
updated: 2022-01-19 01:06:26.565
url: https://halo.imjcker.com/archives/jvmallinone
categories: Java SE
tags: jvm | java虚拟机
---



Java虚拟机学习和优化。

![java](https://imjcker.com:1990/upload/2022/01/java-4b644c9631884d7bbcf7b23fdb17f4e7.png)

## 工具

### 安装jps工具

```shell
yum list | grep jdk-devel

yum -y install xxx
```



## Java 虚拟机

### jstatd 使用

1. 创建policy文件 

```shell  
grant codebase "file:${java.home}/../lib/tools.jar" {   
    permission java.security.AllPermission;
};
```

2. 启动

```shell  
jstatd -J-Djava.security.policy=jstatd.all.policy

jstatd -J-Djava.security.policy=jstatd.all.policy -J-Djava.rmi.server.hostname=192.168.134.128 -p 1099 -J-Djava.rmi.server.logCalls=true
```

3. 本地启动jvisualvm
4. 配置jstatd



### 配置jvm内存

```shell  
@echo off

start "baseservice" java -jar -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=128m -Xms256m -Xmx256m -Xmn256m -Xss256k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC baseservice.jar
```

#### 配置说明

URL：<https://www.javatt.com/p/51429>

```shell
-Xmn1000m -XX:SurvivorRatio=8 
```

Eden区占用80%，To Survivor和From Survivor各占剩余一半即各占10%





https://toutiao.io/posts/132354/app_preview



https://mp.weixin.qq.com/s/fFAv6vgoLszEACO6dmRptA

