---
title: 内网穿透FRP
date: 2021-11-05 17:19:40.794
updated: 2022-01-18 20:43:24.23
url: https://halo.imjcker.com/archives/frp
categories: 极客工具
tags: frp | 内网穿透
---

![frp](https://imjcker.com:1990/upload/2022/01/frp-12b86774738e4c63b2020bdd06316463.png)

# frp

`sudo vi /etc/systemd/system/frpc.service`

## frps.service

```shell
[Unit]
Description=FRP Server Daemon

[Service]
Type=simple
ExecStartPre=-/usr/sbin/setcap cap_net_bind_service=+ep /opt/bin/frps
ExecStart=/root/frp/frps -c /root/frp/frps.ini
Restart=always
RestartSec=20s
User=nobody
PermissionsStartOnly=true

[Install]
WantedBy=multi-user.target
```
### frpc.service

```shell
[Unit]
Description=FRP Client Daemon
After=network.target
Wants=network.target

[Service]
Type=simple
ExecStart=/home/thh/frp/frpc -c /home/thh/frp/frpc.ini
Restart=always
RestartSec=1min
User=nobody

[Install]
WantedBy=multi-user.target
```
### Config

```shell
sudo systemctl start frps.service
sudo systemctl enable frps.service
systemctl status frps.service
```

### frp in docker

```shell
# frps
docker run -d --name frps -e FRP=frps -v /etc/frp/frps.ini:/frp/frps.ini --network host imjcker/frp:latest

# frpc
docker run -d --name frpc -e FRP=frpc -v /etc/frp/frpc.ini:/frp/frpc.ini --network host imjcker/frp:latest
```

### make docker images

通过传参确定启动frps/frpc

```dockerfile
FROM alpine:3.8
MAINTAINER imjcker

WORKDIR /
ENV FRP_VERSION 0.47.0
RUN set -x \
    && wget --no-check-certificate https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz \
    && mkdir -p /frp \
    && tar -xzf frp_${FRP_VERSION}_linux_amd64.tar.gz -C /frp --strip-components=1 \
    && rm -f frp_${FRP_VERSION}_linux_amd64.tar.gz \
    && cd frp

VOLUME /frp

CMD /frp/$FRP -c /frp/$FRP.ini


```
构建镜像：`docker build -t imjcker/frp:latest .`

推送镜像：`docker push imjcker/frp:latest`

