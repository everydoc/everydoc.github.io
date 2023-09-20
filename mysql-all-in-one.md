---
title: MySQL All in One
date: 2021-11-05 23:32:20.925
updated: 2022-01-19 01:23:34.466
url: https://halo.imjcker.com/archives/mysqlallinone
categories: 
tags: mysql
---

![mysql](https://imjcker.com:1990/upload/2022/01/mysql-c536b0758b204bd19bedd520627b6341.png)
1. 安装问题
2. Hello world

&emsp;&emsp;安装5.7.20版本遇到的问题。

安装时说要装很多依赖，又来装了一大堆，最后其它的都装成功了，只剩下Mysql srever怎么装也不行，一只提示如下错误。

```
Action 2:54:32: INSTALL. 
1: MySQL Server 5.7 2: {81B27388-3733-4B65-8D84-AD9C9113B498} 
Action 2:54:32: FindRelatedProducts. Searching for related applications
Action 2:54:32: AppSearch. Searching for installed applications
Action 2:54:32: LaunchConditions. Evaluating launch conditions
This application requires Visual Studio 2013 Redistributable. Please install the Redistributable then run this installer again.
1: MySQL Server 5.7 2: {81B27388-3733-4B65-8D84-AD9C9113B498} 3: 3 
```

意思是缺少**Visual Studio 2013 Redistributable**，因为本人是64位系统，就去微软下载了64位的文件来安装，最后还是不行，后来改为使用32位文件，一次性成功。一下是32位**Visual Studio 2013 Redistributable**下载链接：

[Visual Studio 2013 Redistributable](https://www.microsoft.com/en-in/download/details.aspx?id=40784)

希望后来的同胞少走弯路。

2. docker install mysql

```shell
docker pull mysql:5.7

docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=mysql.123M --restart always -p 3306:3306 mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci
```

3. 配置

   远程连接

   ```shell
   # MySQL 5
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mysql' WITH GRANT OPTION;
   # MySQL 8
   #先创建一个用户
   create user 'imjcker'@'%' identified by 'imjcker';
   #再进行授权
   grant all privileges on *.* to 'imjcker'@'%' with grant option;
   ```

   

4. 创建数据库脚本

```mysql
字符编码: utf8mb4 
排序规则: utf8mb4_general_ci 
create database imjcker default character set utf8mb4 collate utf8mb4_general_ci;
```

5. 备份与恢复 Backup Restore

```shell
mysqldump -u root -p database_nane > file_name.sql;
mysqlimport -u root -p database_name < file_name.sql;
mysql> use db_name;
mysql> source backup-file.sql;
mysql -p -u[user] [database] < backup-file.sql
```

6. 重置密码 Reset Password

with root password

```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```

without root password

```shell
sudo service mysqld stop
sudo mysqld_safe --skip-grant-tables &
mysql -u root
use mysql;
update user set password=password("root") where user='root';
flush privileges;
exit
service mysqld start
```

7. 更改编码 change MySQL default installation encoding

   System: CentOS

```shell
vi /etc/my.cnf
#add server-character-set=utf8 under [mysqlserver]
#add default-character-set=utf8 under [mysql]
#restart mysql
service mysqld restart
#check change:
#1.login mysql
mysql -u root -p
#2.write script
show variables like 'character%';

```

8. MySql内置方法

   GROUP_CONCAT

9. 卸载

**在centos 7操作**

```shell
yum list installed mysql*

yum remove mysql-community-client mysql-community-common mysql-community-libs mysql-community-release mysql-community-server

rm -rf /var/lib/mysql  
rm -rf /etc/my.cnf
rm -rf /usr/share/mysql
```



### xmysql 生成数据库API文档

```shell
npm install -g xmysql

xmysql -h localhost -u root -p password -d database_name


```

![mysql](https://imjcker.com:1990/upload/2022/01/mysql-c536b0758b204bd19bedd520627b6341.png)

## MySQL事务原理

## 面试知识点

