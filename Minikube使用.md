# Minikube使用



## 安装

```shell

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64

sudo install minikube-darwin-arm64 /usr/local/bin/minikube
```



## 使用



```shell
minikube start --driver=docker --alsologtostderr
```



### 桌面

```shell
minikube dashboard
```









## 参考文献

1. https://www.jianshu.com/p/ef400bfea973 Minikube快速入门手册
2. xxx