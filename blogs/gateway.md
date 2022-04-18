# 万能网关
对于大多数个人开发者来说，开发并部署一个自己的产品是一件很伤钱的事，不管是哪个云服务，都不是起步阶段的我们能够承受的，为了搭建我们的第一个产品[EveryDoc](https://pdf.everydoc.icu)，我们基于已有的知识和能力，搭建了一个万能网关。

## 原理

nginx + frps

1. 所有网络从nginx进入
2. nginx转发请求到frps

## nginx
```shell
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    proxy_buffering     off;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;    

    server {
        listen 443 ssl;
        server_name imjcker.com  *.imjcker.com;
        client_max_body_size 1G;
        client_header_buffer_size 8k;
        large_client_header_buffers 8 128k;
        #include /etc/nginx/default.d/*.conf;
        location / {
            proxy_pass    http://localhost:8080;
            # add_header Access-Control-Allow-Origin *;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    
        ssl_certificate /root/.acme.sh/imjcker.com/fullchain.cer;
        ssl_certificate_key /root/.acme.sh/imjcker.com/imjcker.com.key; 
    }

    server {
        listen       8089;
        server_name  everydoc;

        root    /usr/share/nginx/_site;
        include /etc/nginx/default.d/*.conf;

        location / {
            if (!-e $request_filename) {
                rewrite ^(.*)$ /index.html?s=$1 last;
                break;
            }
        }

        error_page 404 /404.html;
        location = /404.html {
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }

    server {
        listen       80;
        server_name imjcker.com  *.imjcker.com;
        return 301 https://$host$request_uri;
    }
}

```

## frps

