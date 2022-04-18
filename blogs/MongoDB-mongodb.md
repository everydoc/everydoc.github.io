---
title: MongoDB
date: 2021-11-05 23:32:18.613
updated: 2021-11-05 23:32:18.613
url: https://halo.imjcker.com/archives/mongodb
categories: 
tags: mongoDB
---


 MongoDB 搭建使用记录。


```shell
docker run -d --name mongo-server -p 27017:27017 \
-v /Users/alanturing/mongo/configdb:/data/configdb -v /Users/alanturing/mongo/db:/data/db \
-e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=mongo \
mongo:latest
```



进入mongo容器

```shell
docker exec -it mongo-server bash
```