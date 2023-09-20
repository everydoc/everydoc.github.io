---
title: 杂记_近期笔记.md
date: 2021-11-05 23:33:06.455
updated: 2021-11-05 23:33:06.455
url: https://halo.imjcker.com/archives/杂记近期笔记md
categories: 
tags: 
---

--------------------------- | ---------------- |
|                                |                  |
|                                |                  |
|                                |                  |



## MyBatis

### 插入数据，获取主键

```xml
<insert id="insertAndGetId" useGeneratedKeys="true" keyProperty="userId" parameterType="com.chenzhou.mybatis.User">
```

### 批量操作

批量删除

<delete id="batchDeleteUser">
​        delete from t_user where id in (
​        <foreach collection="ids" item="id" separator=",">
​            \#{id}
​        </foreach>
​        )  

 </delete>

<!-- 第二种批量删除的写法 -->

<!-- open表示该语句以什么开始，close表示以什么结束 -->

<delete id="batchDeleteUser">
​        delete from t_user where id in 
​        <foreach collection="ids" item="id" separator="," open="(" close=")">
​            \#{id}
​        </foreach>

</delete>

### Mybatis Generator

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <context id="oracle_lajd" targetRuntime="MyBatis3">
        <!-- 生成的pojo，将implements Serializable-->
        <!--<plugin type="org.mybatis.generator.plugins.SerializablePlugin"></plugin>-->
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!-- 数据库链接URL、用户名、密码 -->
        <jdbcConnection driverClass="oracle.jdbc.driver.OracleDriver"
                        connectionURL="jdbc:oracle:thin:@192.168.1.241:1521:jcydb"
                        userId="tyyw"
                        password="tyyw">
        </jdbcConnection>

        <!--
        默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
            true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal
        -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!--
        生成model模型，对应的包路径，以及文件存放路径(targetProject)，targetProject可以指定具体的路径,如./src/main/java，
        也可以使用“MAVEN”来自动生成，这样生成的代码会在target/generatord-source目录下
        -->
        <!--<javaModelGenerator targetPackage="com.joey.mybaties.test.pojo" targetProject="MAVEN">-->
        <javaModelGenerator targetPackage="com.cyvation.lajd.pojo" targetProject="./src/main/java">
            <property name="enableSubPackages" value="true"/>
            <!-- 从数据库返回的值被清理前后的空格  -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!--对应的mapper.xml文件  -->
        <sqlMapGenerator targetPackage="com.cyvation.lajd.dao" targetProject="./src/main/resources">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 对应的Mapper接口类文件 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.cyvation.lajd.dao" targetProject="./src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>


        <!-- 列出要生成代码的所有表，这里配置的是不生成Example文件 -->

        <table tableName="XT_SSJD_LAJD_LAAYGX" domainObjectName="XtSsjdLajdLaaygx"
               enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false" >
            <property name="useActualColumnNames" value="false"/>
        </table>

        
    </context>
</generatorConfiguration>
```

## Vue.js

### open sources

* https://github.com/vue-bulma/vue-admin
* https://github.com/coreui/coreui-free-bootstrap-admin-template
* https://github.com/PanJiaChen/vue-element-admin
* https://github.com/sls-admin/sls-admin-vue

### how to learn

* write a demo
* add features 

## 持续集成(CI)

### install gitlab

#### installation

docker pull gitlab/gitlab-ce:latest
#### launch

docker run  --name gitlab --detach --hostname gitlab.localhost.com  --publish 9443:443 --publish 9080:80 --publish 9022:22 --restart always -v $PWD/config:/etc/gitlab -v $PWD/logs:/var/log/gitlab -v $PWD/data:/var/opt/gitlab gitlab/gitlab-ce:latest
### install nginx

#### installation

docker pull nginx
#### launch

docker run --name nginx -p 80:80 -v $PWD/html:/usr/share/nginx/html -v $PWD/logs:/var/log/nginx -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -d nginx  

## 设计模式

1. 代理模式是对象的结构模式。代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。

2. 使用工厂模式关心的是是否达到了降低耦合的目的，而不用关心到底使用的是“简单工厂模式，工厂模式，抽象工厂模式”中的哪一种。
3. 

## Effective Java

1.  对于非公有的方法，通常应该使用assertion来检查他们的参数，而不使用正常的检查语句
2.  只要能够不创建对象，就不要创建多余的对象。只要需要创建对象，就不要吝啬地创建它。它们并不矛盾，前者的目的是为了尽可能减少资源的占用，提高运行效率，后者是为了安全性（保护性拷贝）。
3.  

## MySql

### 内置方法

#### GROUP_CONCAT

## Oracle 操作

### 导库

```sql
--创建表空间
CREATE TABLESPACE jcker DATAFILE 'C:\app\Administrator\oradata\jcker\jcker.dbf' size 500M autoextend on next 10M maxsize 30G LOGGING EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
--创建用户并分配权限
create user jcker identified by jcker default tablespace jcker temporary tablespace temp;
alter user jcker quota unlimited on jcker quota 0M on system;
--设置用户密码永不过期
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
GRANT connect, resource to jcker;
GRANT create public synonym to jcker;
GRANT create synonym to jcker;
GRANT create any table to jcker;
GRANT create any view to jcker;
GRANT debug connect session to jcker;
GRANT debug any PROCEDURE to jcker;
GRANT create tablespace to jcker;
GRANT alter tablespace to jcker;
GRANT create any directory to jcker;
GRANT create database link to jcker;
GRANT execute on dbms_lock to jcker;
GRANT datapump_exp_full_database to jcker;
GRANT datapump_imp_full_database to jcker;
-- 控制台执行导库
imp userid=jcker/jcker full=y file=C:\app\Administrator\oradata\jcker\KSS20180710.dmp ignore=Y log=C:\Users\Administrator\Desktop\log.log

```

### Oracle字符编码查询

```sql 
SELECT t.* FROM nls_database_parameters t;
SELECT t.* FROM nls_instance_parameters t;
SELECT t.* FROM v$version t;


SELECT userenv('language') from dual;--查询Oracle server字符集

SELECT nls_charset_name(to_number('0354','xxxx')) from dual;--通过查询dmp文件的二进制格式的第2，3位，判断dmp文件的编码。



alter system set "_system_trig_enabled"=false;
```



## Windows 启动Java脚本

```shell
@echo off
title 窗口标题
echo 打印输出

java -jar scbp-ycsd-service.jar

exit
```

## 快速添加Windows的hosts文件

```shell
@echo off

echo 127.0.0.1:9001 www.jcker.org >> C:\WINDOWS\system32\drivers\etc\hosts

exit
```



## Git 操作

检查本地git地址：

```bash
git remote -v
```

更改本地版本路径：

```bash
git remote set-url origin https://github.com/imjcker/jcker.git
```

## GitLab配置

### 配置url

## Windows 内外网同时使用配置路由

> >在命令提示符里面输入：route add -p 10.173.0.0 mask 255.255.0.0 192.168.1.1
> >这样设置就是把有线连接需要网络的时候就会连接到服务器：10.173.0.0（无线wifi的网段
>
> route add -p 172.20.10.1 mask 255.255.0.0 192.168.2.1
>
>
> 2、在命令提示符里输入：route add 172.18.0.0 mask 255.255.0.0 172.18.100.254
> 这里给大家解释一下，这套命令的作用是把所有访问内网172.18.xxx.xxx的请求转发给内网网关172.18.100.254。
> 如果你们的内网还有其它网段的话就照上面的格式依次添加即可。比如：你们公司的内网还有这样的地址172.16.1.25，
> 你就可以这样写：route add 172.16.0.0 mask 255.255.0.0 172.18.100.254



## Spring boot

### 自定义yml配置

1. 创建一个配置类如：DemoConfigProperties, 类结构对应yml配置文件结构

   > ```java
   > //root为跟节点，就好型spring项下配置的其实节点spring
   > @ConfigurationProperties(prefix = "root")
   > public class DemoConfigProperties {
   >     private String sub1;
   >     private int sub2;
   > 
   >     //getters and setters
   > }
   > ```
   >
   > 对应的yml结构如下：
   >
   > ```yaml
   > root:
   >   sub1: xxx
   >   sub2: yyy
   > ```
   >
   >

2. 在一个加有@Configuration的类（最好加一个单独的类，以区分其作用）上加上启动自定义注解：

   > ```java
   > @EnableConfigurationProperties(DemoConfigProperties.class)
   > //也可直接加在spring boot 启动类上
   > ```
   >
   >

3. 使用配置信息，在spring 容器管理的任何bean中，都可以直接使用@Aurowired注入这个配置。