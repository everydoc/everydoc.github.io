---
title: idea
date: 2021-11-05 23:31:20.973
updated: 2022-01-18 21:48:21.196
url: https://halo.imjcker.com/archives/ideamd
categories: 
tags: 
---

# IntelliJ IDEA常用快捷键



IntelliJ IDEA 使用心得及操作记录。

>  Alt + 0 Message
>  Alt + 1 project 
>  Alt + 9 subversion
>  Alt + Insert generate code like getter setter
>  Ctrl + Alt + T surround with...
>  Ctrl + Shift + up/down move line up/down
>
>  Ctrl + Alt +B go to implementation  
>
>  shift + F9	快速启动Debug  
>
>  Ctrl + Shift + U	大小写转换

## 插件

### CodeGlance

查看代码结构

### Jrebel

热加载，节约开发时间

This application still working today, but for then reason of copyright, I was not allow to publish this post, this is a way to work around if possible. LMAO...😂

**enjoy::)** 

http://jrebel.imjcker.com/uuid
replace uuid with a real one，[generate UUID online](https://1024tools.com/uuid)

this URL may not work, cause it is running on a private PC, :/ 😂，if possible, please set your own license server.



run license server in docker container, this may be the easiest way to deploy it. 

```shell
docker run -d --name jrebel -p 9090:9090 --restart always imjcker/jrebel:latest
```



if you do not use docker, there is another way to deploy. clone the repository then run it as a stand alone spring boot application. 

1. clone 

   ```shell
   git clone https://github.com/imjcker/jrebel-license-server
   ```

2. build

   ```shell
   mvn clean package
   ```

3. run

   ```shell
   java -jar jrebel-1.0-SNAPSHOT-jar-with-dependencies.jar
   ```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200110090308294.jpg)

如果你觉得有用，可以给这个项目[jrebel-license-server](https://github.com/imjcker/jrebel-license-server)打个小星星哟😊



我开了个公众号，没怎么维护，😂

### Free MyBatis Plugin

实用的MyBatis插件



### Lombok

代码简洁必用插件



## 编码

### 设置注解

**class 注解**

> Editor-File and Code Templates-Files-Class
>
> ```java
> /**
>  * TODO
>  * @author ${USER} (https://github.com/imjcker)
>  * @version ${YEAR}-${MONTH}-${DAY}
> */
> ```