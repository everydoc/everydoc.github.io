---
title: Jenkins安装
date: 2022-01-18 19:24:43.839
updated: 2022-01-18 19:24:43.839
url: https://halo.imjcker.com/archives/jenkins-an-zhuang
categories: 极客工具
tags: Jenkins |  CI
---

![jenkins](https://imjcker.com:1990/upload/2022/01/jenkins-fe4fcd3323844fc6b41d58c87551d7af.png)

基于docker搭建Jenkins持续集成环境。

## 安装Jenkins

```shell
# 创建挂在目录
mkdir /data/jenkins_home
# 设置读写权限
chmod -R 777 /data/jenkins_home
# 拉取并启动jenkins
docker run -d --name jenkins -p 8087:8080 -p 50000:50000 --restart always -v /data/jenkins_home:/var/jenkins_home jenkins/jenkins:latest
# 获取初始化密码
docker logs jenkins
# a1bd4cfc7d354322a062eedc5840e346
```

### 手动安装

```shell
wget https://pkg.jenkins.io/redhat-stable/jenkins-2.222.1-1.1.noarch.rpm
rpm -ivh jenkins-2.222.1-1.1.noarch.rpm
```

#### 配置

```shell
vim /etc/sysconfig/jenkins
#监听端口
JENKINS_PORT="8080"
JENKINS_USER="root"
## 目录权限
chown -R root:root /var/lib/jenkins
chown -R root:root /var/cache/jenkins
chown -R root:root /var/log/jenkins
## 重启 
systemctl restart jenkins

```

