---
title: ApiSix.md
date: 2021-11-05 17:19:32.951
updated: 2021-11-05 17:19:32.951
url: https://halo.imjcker.com/archives/apisixmd
categories: 
tags: 
---

# ApiSix部署使用

## 1.安装部署

1. 安装epel

   ```shell
   rpm -ivh epel-release-latest-7.noarch.rpm
   ```

   

2. 安装etcd

   ```shell
   tar -xvf etcd-v3.4.13-linux-amd64.tar.gz
   cp -a etcd-v3.4.13-linux-amd64/etcd etcd-v3.4.13-linux-amd64/etcdctl /usr/bin/
   nohup etcd >/var/log/etcd.log 2>&1 &
   
   # 开机启动
   
   ```

   

3. 安装openresty

   ```shell
   rpm -ivh openresty-*.rpm
   ```

   

4. 安装apisix

   ```shell
   yum -y install apisix-2.1-0.el7.noarch.rpm
   ```

   

5. 启动

   ```shell
   apisix version
   apisix start
   ```

   

## 2.安装dashboard

```shell
$ sudo yum install -y https://github.com/apache/apisix-dashboard/releases/download/v2.6/apisix-dashboard-2.6-0.x86_64.rpm

$ sudo nohup manager-api -p /usr/local/apisix/dashboard/ &
```





## 3.配置注册中心

对接Eureka