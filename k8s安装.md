# k8s安装

## 1.准备工作



### 修改克隆机器配置

1. MAC 地址
2. 网卡uuid
3. product_uuid /sys/class/dmi/id/**product_uuid**



### 配置源

```shell
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```



### 网络配置

```shell
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```



### 关闭selinux

```shell
vi /etc/selinux/config
# 改为 disabled
```

### 关闭swap

```shell
swapoff -a
sed -ri 's/.*swap.*/#&/' /etc/fstab
```

### 开通防火墙端口

在测试环境可以关闭防火墙，生产环境不推荐。

地址：https://kubernetes.io/zh-cn/docs/reference/networking/ports-and-protocols/

### 设置cgroup

```shell
# 设置docker
vi /etc/docker/daemon.json
"exec-opts": ["native.cgroupdriver=systemd"]
# 设置kubelet，1.22版本后默认systemd，所以可以忽略
sed -i "s/cgroup-driver=systemd/cgroup-driver=cgroupfs/g" /usr/lib/systemd/system/kubelet.service.d/10-kubeadm.conf
```



## 2.安装kube3件套

```shell
yum -y install kubeadm kubelet kubectl && systemctl enable kubelet
```

### 安装自动补全

```shell
yum -y install bash-completion

# vi .bashrc 使生效
if [ -f /etc/bash_completion ]; then

. /etc/bash_completion
fi

```

### 代理设置

```shell
mkdir /etc/systemd/system/docker.service.d
vi /etc/systemd/system/docker.service.d/proxy.conf
# 添加如下内容
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:20171"
Environment="HTTPS_PROXY=http://127.0.0.1:20171"
Environment="NO_PROXY=localhost,127.0.0.1,hub.docker.com,imjcker.synology.me"
```



## 3.拉取镜像

### 编写脚本

```shell
kubeadm config images list

registry.k8s.io/kube-apiserver:v1.26.3
registry.k8s.io/kube-controller-manager:v1.26.3
registry.k8s.io/kube-scheduler:v1.26.3
registry.k8s.io/kube-proxy:v1.26.3
registry.k8s.io/pause:3.9
registry.k8s.io/etcd:3.5.6-0
registry.k8s.io/coredns/coredns:v1.9.3
```



```shell
#!/bin/bash
# 通过阿里云拉取镜像，并重新tag成指定镜像名称。
set -e

KUBE_VERSION=v1.26.3
KUBE_PAUSE_VERSION=3.9
ETCD_VERSION=3.5.6-0
CORE_DNS_VERSION=v1.9.3

GCR_URL=registry.k8s.io
ALIYUN_URL=registry.cn-hangzhou.aliyuncs.com/google_containers

images=(kube-proxy:${KUBE_VERSION}
kube-scheduler:${KUBE_VERSION}
kube-controller-manager:${KUBE_VERSION}
kube-apiserver:${KUBE_VERSION}
pause:${KUBE_PAUSE_VERSION}
etcd:${ETCD_VERSION}
coredns:${CORE_DNS_VERSION})

for imageName in ${images[@]} ; do
  docker pull $ALIYUN_URL/$imageName
  docker tag  $ALIYUN_URL/$imageName $GCR_URL/$imageName
  docker rmi $ALIYUN_URL/$imageName
done

```



## 4.端口开放

```shell
firewall-cmd --zone=public \
--add-port = 6443/tcp \
--add-port = 2379-2380/tcp \
--add-port = 10250/tcp \
--add-port = 10257/tcp \
--add-port = 10259/tcp \
--add-port = 9099/tcp \
--add-port = 30000-32767/tcp \
--permanent

firewall-cmd --reload
```





## 5.工具安装



## 6.搭建集群

```shell
kubeadm init \
--apiserver-advertise-address=192.168.0.107 \
--image-repository registry.aliyuncs.com/google_containers \
--kubernetes-version v1.26.3 \
--service-cidr=10.96.0.0/12 \
--pod-network-cidr=10.244.0.0/16 \
--control-plane-endpoint=m1.imjcker.com \
--v=9
```

```shell
kubeadm init \
--apiserver-advertise-address=192.168.0.107 \
--kubernetes-version v1.26.3 \
--service-cidr=10.96.0.0/12 \
--pod-network-cidr=10.244.0.0/16 \
--control-plane-endpoint=m1.imjcker.com \
--v=9
```



kubeadm 不支持将没有 `--control-plane-endpoint` 参数的单个控制平面集群转换为高可用性集群。

### 成功结果

```shell
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of control-plane nodes by copying certificate authorities
and service account keys on each node and then running the following as root:

  kubeadm join m1.imjcker.com:6443 --token j43lew.ds7dgownhkakiu6t \
	--discovery-token-ca-cert-hash sha256:d341d4a0b8353707e2b4c83acd951517057edaf0e676fe23f2fe278c0901ddff \
	--control-plane

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join m1.imjcker.com:6443 --token j43lew.ds7dgownhkakiu6t \
	--discovery-token-ca-cert-hash sha256:d341d4a0b8353707e2b4c83acd951517057edaf0e676fe23f2fe278c0901ddff
	
```

## 7.添加节点

```shell
kubeadm join m1.imjcker.com:6443 --token j43lew.ds7dgownhkakiu6t --discovery-token-ca-cert-hash sha256:d341d4a0b8353707e2b4c83acd951517057edaf0e676fe23f2fe278c0901ddff
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```



## 8.配置CNI插件

### 开通calico-node端口

```shell
firewall-cmd --add-port=9099/tcp --zone=public --permanent
firewall-cmd --reload
```



部署节点通信网络，在master节点执行即可

~~kubectl apply -f https://docs.projectcalico.org/v3.10/manifests/calico.yaml~~

```shell
# 直接执行不行就在外网下载后执行
wget https://docs.tigera.io/archive/v3.25/manifests/calico.yaml
# 通过关键字`192`找到cidr配置，修改IP为 kubeadm init 初始化的--pod-network-cidr地址。
kubectl apply -f calico.yaml
```

查看pods状态

```shell
# 查看
kubectl get pods -n kube-system
# 监听
watch -n 1 kubectl get pod -n kube-system -o wide
```

查看节点状态

```shell
kubectl get nodes
```



### Ingress-nginx

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.6.4/deploy/static/provider/cloud/deploy.yaml


```



## 9.安装UI管理

### Kubernetes dashboard

项目地址：https://github.com/kubernetes/dashboard

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

```

```yaml
# 自定义dashboard
apiVersion: v1
kind: Service
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
spec:
  type: NodePort
  ports:
    - port: 443
      targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
```



#### 配置用户权限

kubernetes-dashboard-admin.yaml

```yaml
---
# ServiceAccount
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin
  namespace: kubernetes-dashboard
---
# ClusterRoleBinding  
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin
  namespace: kubernetes-dashboard
```



`kubectl apply -f kubernetes-dashboard-admin.yaml`

#### Bearer token

`kubectl -n kubernetes-dashboard create token admin`

```shell
eyJhbGciOiJSUzI1NiIsImtpZCI6IlhEZFJoX1IyMFl3T1BMOTdYeVFoSkstc2lOZ1dodF9vX1h6ODZvNEtKUW8ifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc5NDIxODE5LCJpYXQiOjE2Nzk0MTgyMTksImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJhZG1pbiIsInVpZCI6ImVjMDEyNTgwLWEyYTQtNGZiMS05ODYwLTM4MTU2OGNmNzVkZCJ9fSwibmJmIjoxNjc5NDE4MjE5LCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4ifQ.1AQDolBaosI2GRGOUDQECFY90sAH1Hdc1HZab-zYFtoDkUEu_dYivM_fkvAvX9SjQ8J7al1WtPncXD9144XQn14Gu9AGQSXgLlBY0jYlYAObzxT0-pPcdAwQ6BEVqVRwz3A4sUITVGta-Nqcgr81R2vQDY_7cQXQ4IA4FyP7wpuVc1xdqY0shQEPU_1_Q6ac_2AfXGLCR49_sFd1hgF2MGwgR7MgUZYS6m4G6tt6hqUzNUMBCURr2w_BHyu3i1LSCvS64wyOGJICRyxTt6UAx7kZb8KixDVf_0fGQFuc3wZ8iboK6husu6f11tx5RYDEcMX34oEsoF3S8y7F-U3jNQ
```





### kubesphere



### kuboard



## 9.常见错误

1. [ERROR CRI]: container runtime is not running

   一般情况是kubelet启动失败，主要原因是默认的config.toml配置文件的配置项cri插件导致，可以选择删除或注释掉后重启containerd

   > ```shell
   > rm /etc/containerd/config.toml
   > systemctl restart containerd
   > kubeadm init
   > ```

2. xxx

   > ```shell
   > sudo kubeadm reset
   > rm -rf .kube/
   > sudo rm -rf /etc/kubernetes/
   > sudo rm -rf /var/lib/kubelet/
   > sudo rm -rf /var/lib/etcd
   > ```

3. Timeout 

   > Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s

   1. 检查是否关闭selinux、swap
   2. 检察kubelet是否启动，检察kubelet日志：journalctl -xefu kubelet
   3. 1.26.3报错，发现其依赖的pause版本不是对应的3.9而是3.6，单独拉取即可

4. Calico CNI插件问题

   > coredns,calico-node 等一无法启动，可能是calico的端口为开放，9099.

5. 集群无辜崩溃
   1. 第二天不能用了，kubectl都不能用



6. calico-kube-controllers

   > calico插件安装后一直无法启动，
   >
   > Failed to initialize datastore error=Get "https://10.96.0.1:443/apis/crd.projectcalico.org/v1/clusterinformations/default": dial tcp 10.96.0.1:443: connect: no route to host

   打开master的443

7. 报错--container-runtime-endpoint未设置

   > https://blog.csdn.net/CarlZ42/article/details/125229815



## 后续

### 后续打印join链接

```shell
kubeadm token create --print-join-command
```



## 参考文献

1. https://blog.51cto.com/u_15287666/5269619
2. https://blog.csdn.net/CarlZ42/article/details/125229815





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

