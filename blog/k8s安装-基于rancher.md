# k8s安装-基于rancher



## 安装v2ray

基于不可描述原因，安装v2ray

```shell
docker run -d --name v2raya \
--restart=always --privileged --network=host \
-v /lib/modules:/lib/modules:ro \
-v /etc/resolv.conf:/etc/resolv.conf \
-v /etc/v2raya:/etc/v2raya mzz2017/v2raya
```

> https://usa.imjcker.com/s/usa.imjcker.com_aeab1cbf387ec0fe002026bf7558f5cb



## 安装rancher

```shell
docker run -d --name rancher --privileged --restart=unless-stopped \
  -p 9090:80 -p 9443:443 \
  rancher/rancher:latest
  
```

