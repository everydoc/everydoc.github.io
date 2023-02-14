---
title: Jenkins 环境搭建
date: 2021-11-05 17:19:35.889
updated: 2021-11-05 17:19:35.889
url: https://halo.imjcker.com/archives/jenkins环境搭建
categories: 
tags: Jenkins
---

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

# Jenkins

## for simple production

```shell
docker volume create jenkins-data
docker run --name jenkins \
           --detach \
           -p 50000:50000 \
           -p 8080:8080 \
           -v jenkins-data:/var/jenkins_home \
           jenkins/jenkins:latest
# If run for the first time, just run the following to get the admin
# password once it has finished starting
docker exec jenkins bash -c 'cat $JENKINS_HOME/secrets/initialAdminPassword'
# in my case i got: 661c5ac62ee64c5c9ce5e2aa5df01dd1
```

## upgrade

```shell
docker stop jenkins
docker rm jenkins # just temporarily docker rename it instead if that makes you worried
docker run --name jenkins \
           --detach \
           -p 50000:50000 \
           -p 8080:8080 \
           -v jenkins-data:/var/jenkins_home \
           jenkins/jenkins:2.121.3
```

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

## 使用

配置国内镜像源

```shell
# 官方地址
https://updates.jenkins.io/update-center.json
# 国内镜像
http://mirror.xmission.com/jenkins/updates/update-center.json
```