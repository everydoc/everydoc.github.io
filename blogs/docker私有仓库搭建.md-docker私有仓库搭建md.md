---
title: docker私有仓库搭建.md
date: 2021-11-05 17:19:38.319
updated: 2021-11-05 17:19:38.319
url: https://halo.imjcker.com/archives/docker私有仓库搭建md
categories: 
tags: 
---

# docker 私有仓库搭建

搭建一个私有的docker仓库。

## 下载镜像

```shell
docker pull registry
```



## 启动脚本

```shell
docker run --name my-registry -d --restart always -p 8090:5000 docker.io/registry 
```



## 测试

### 拉取测试镜像

```shell
docker pull hello-world
```

### 标记镜像

```shell
docker tag hello-world:latest nas.imjcker.com:8090/hello-world
```

### 推送镜像

```shell
docker push nas.imjcker.com:8090/helloworld
```

### 查看镜像

```shell
curl http://nas.imjcker.com:8090/v2/
```