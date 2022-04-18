---
title: CDH.md
date: 2021-11-05 17:19:35.716
updated: 2021-11-05 17:19:35.716
url: https://halo.imjcker.com/archives/cdhmd
categories: 
tags: 
---

# CDH搭建



## 初始化系统

1. 安装常用软件

  ```shell
  yum install wget iptables-services telnet net-tools git curl unzip sysstat lsof ntpdate lrzsz vim  -y
  ```

  

2. 安装ntp时间服务器并启动

  ```shell
  yum install ntp -y
  systemctl start ntpd
  systemctl enable ntpd
  ```

  

3. 为了后续方便操作，先关闭防火墙并禁止开机启动(iptables为上图安装,firewalld为centos7默认防火墙)

  ```shell
  systemctl stop iptables
  systemctl disable iptables
  systemctl stop firewalld
  systemctl disable firewalld
  ```

  

## 安装JDK和MySQL连接

1. jdk

   ```shell
   rpm -ivh oracle-j2sdk1.8-1.8.0+update181-1.x86_64.rpm 
   #配置环境变量
   echo "export JAVA_HOME=/usr/java/jdk1.8.0_181-cloudera"  >> /etc/profile
   echo "export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib"  >> /etc/profile
   echo "export PATH=$PATH:$JAVA_HOME/bin"  >> /etc/profile
   
   source /etc/profile   #声明
   java -version         #查看版本
   ```

2. Mysql-connect

   ```shell
   mkdir /usr/share/java/  && cd /usr/share/java/
   wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz
   ```



## 配置hosts

```shell
192.168.0.111 nas1.imjcker.com nas1
192.168.0.112 nas2.imjcker.com nas2
192.168.0.113 nas3.imjcker.com nas3
```

## 安装mysql



## 配置数据库

```sql
CREATE USER 'scm'@'%' IDENTIFIED BY 'mysql@scm';

CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON scm.* TO 'scm'@'%' IDENTIFIED BY 'mysql@scm';

create user 'amon'@'%' identified by 'mysql@amon';
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON amon.* TO 'amon'@'%' IDENTIFIED BY 'mysql@amon';

create user 'rman'@'%' identified by 'mysql@rman';
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON rman.* TO 'rman'@'%' IDENTIFIED BY 'mysql@rman';

create user 'metastore'@'%' identified by 'mysql@metastore';
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON metastore.* TO 'metastore'@'%' IDENTIFIED BY 'mysql@metastore';

create user 'sentry'@'%' identified by 'mysql@sentry';
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry'@'%' IDENTIFIED BY 'mysql@sentry';

create user 'nav'@'%' identified by 'mysql@nav';
CREATE DATABASE nav DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON nav.* TO 'nav'@'%' IDENTIFIED BY 'nav';

create user 'navms'@'%' identified by 'mysql@navms';
CREATE DATABASE navms DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON navms.* TO 'navms'@'%' IDENTIFIED BY 'mysql@navms';

create user 'oozie'@'%' identified by 'mysql@oozie';
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie'@'%' IDENTIFIED BY 'mysql@oozie';

create user 'hue'@'%' identified by 'mysql@hue';
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON hue.* TO 'hue'@'%' IDENTIFIED BY 'mysql@hue';

```



## 安装manager



```shell
yum -y localinstall cloudera-manager-daemons-6.1.1-853290.el7.x86_64.rpm
yum -y localinstall cloudera-manager-agent-6.1.1-853290.el7.x86_64.rpm
```