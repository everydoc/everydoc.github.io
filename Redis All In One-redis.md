# Redis All In One



![redis](https://imjcker.com:1990/upload/2022/01/redis-4e8e675dbe5c443da452c2946c4ba700.png)
1. 安装gcc

2. 安装reids

下载redis压缩包

<https://redis.io/download>

3. 编译安装

```shell
make
```

安装后的redis-server在src目录下

redis-cli也在src目录下

4. docker 安装

   ```shell
   docker pull redis:latest
   
   docker run -d --name redis -p 6379:6379 --restart always -v ~/redis/data:/data redis redis-server --appendonly yes
   ```

4. 安装redis commander管理工具

   ```shell
   docker run --name redis-commander -d --restart always --env REDIS_HOSTS=localhost -p 8081:8081 rediscommander/redis-commander:latest
     
   # or use npm install   
   npm install -g redis-commander
   ```

   ![redis commander](/assets/2020/redis.commander.index.png)

4. 开机启动

```shel
[Unit]
Description=The redis-server Process Manager
After=syslog.target network.target

[Service]
Type=forking
PIDFile=/var/run/redis_6379.pid
ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
ExecReload=/bin/kill -USR2 $MAINPID
ExecStop=/bin/kill -SIGINT $MAINPID

[Install]
WantedBy=multi-user.target
```

## 集群搭建

每台服务器配置

6. 主从配置

redis-6379.conf配置文件内容如下：

```null
bind 127.0.0.1
port 6379
daemonize yes
logfile "6379.log"
dbfilename "dump-6379.rdb"
```

redis-6380.conf配置文件内容如下：

```null
bind 127.0.0.1
port 6380
daemonize yes
logfile "6380.log"
dbfilename "dump-6380.rdb"
slaveof 127.0.0.1 6379
```

7. 集群配置

启动

```shell
redis/src/redis-trib.rb create --replicas 1 create 172.32.15.112:6379 172.32.15.113:6379 172.32.15.114:6379 172.32.14.228:6379 172.32.14.229:6379 172.32.14.230:6379
```



```shell
redis/src/redis-cli --cluster create 172.32.15.112:6379 172.32.15.113:6379 172.32.15.114:6379 172.32.14.228:6379 172.32.14.229:6379 172.32.14.230:6379 --cluster-replicas 1

```

## 面试知识点

