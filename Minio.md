# 搭建minio

```shell

docker run -d \
  -p 9000:9000 \
  -p 9001:9001 \
  --name minio \
  -v ~/minio/data:/data \
  -e "MINIO_ROOT_USER=root" \
  -e "MINIO_ROOT_PASSWORD=minio.123M" \
  quay.io/minio/minio server /data --console-address ":9001"
  
```





### 配置客户端

```shell
wget https://dl.minio.io/client/mc/release/linux-amd64/mc

mc config host add minio http://localhost:9000 everydoc everydoc.icu --api s3v4

mc policy set download minio/pdf

```

