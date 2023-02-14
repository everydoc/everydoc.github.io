---
title: Docker All in One
date: 2021-11-05 17:19:38.553
updated: 2022-01-19 00:54:46.684
url: https://halo.imjcker.com/archives/dockerallinone
categories: 
tags: docker
---

随着Docker技术的不断成熟，越来越多的企业开始考虑使用Docker。

## docker的好处和优势

Docker有很多的优势，本文主要讲述了Docker的五个最重要优势，即持续集成、版本控制、可移植性、隔离性和安全性。对于Docker，应该不需要进行详细的介绍了。它是最火热的开源项目之一，通过在容器中增加一个抽象层（a layer of abstraction），就可以将应用程序部署到容器中。在看似稳定而成熟的场景下，使用Docker的好处越来越多。

### 持续部署与测试

Docker在开发与运维的世界中具有极大的吸引力，因为它能保持跨环境的一致性。在开发与发布的生命周期中，不同的环境具有细微的不同，这些差异可能是由于不同安装包的版本和依赖关系引起的。然而，Docker可以通过确保从开发到产品发布整个过程环境的一致性来解决这个问题。Docker容器通过相关配置，保持容器内部所有的配置和依赖关系始终不变。最终，你可以在开发到产品发布的整个过程中使用相同的容器来确保没有任何差异或者人工干预。

使用Docker，你还可以确保开发者不需要配置完全相同的产品环境，他们可以在他们自己的系统上通VirtualBox建立虚拟机来运行Docker容器。

Docker的魅力在于它同样可以让你在亚马逊，阿里云，腾讯云，航天等云平台实例上运行相同的容器。如果你需要在一个产品发布周期中完成一次升级，你可以很容易地将需要变更的东西放到Docker容器中，测试它们，并且使你已经存在的容器执行相同的变更。这种灵活性就是使用Docker的一个主要好处。和标准部署与集成过程一样，Docker可以让你构建、测试和发布镜像，这个镜像可以跨多个服务器进行部署。哪怕安装一个新的安全补丁，整个过程也是一样的。你可以安装补丁，然后测试它，并且将这个补丁发布到产品中。

### 多云平台

Docker最大的好处之一就是可移植性。在过去的几年里，所有主流的云计算提供商，包括阿里，百度，腾讯，华为，都将Docker融入到他们的平台并增加了各自的支持。Docker容器能还能运行在亚马逊的EC2实例、谷歌的GCP实例、Rackspace服务器或者VirtualBox这些提供主机操作系统的平台上。举例来说，如果运行在亚马逊EC2实例上的Docker容器能够很容易地移植到其他几个平台上，比如说VirtualBox，并且达到类似的一致性和功能性，那这将允许你从基础设施层中抽象出来。除了AWS和GCP，Docker在其他不同的IaaS提供商也运行的非常好，例如微软的Azure、OpenStack和可以被具有不同配置的管理者所使用的Chef、Puppet、Ansible等。

### 环境标准化和版本控制

通过上面的讨论，Docker容器可以在不同的开发与产品发布生命周期中确保一致性，进而标准化你的环境。除此之外，Docker容器还可以像git仓库一样，可以让你提交变更到Docker镜像中并通过不同的版本来管理它们。设想如果你因为完成了一个组件的升级而导致你整个环境都损坏了，Docker可以让你轻松地回滚到这个镜像的前一个版本。这整个过程可以在几分钟内完成，如果和虚拟机的备份或者镜像创建流程对比，那Docker算相当快的，它可以让你快速地进行复制和实现冗余。此外，启动Docker就和运行一个进程一样快。

### 隔离性

Docker可以确保你的应用程序与资源是分隔开的。

我们考虑这样一个场景，你在你的虚拟机中运行了很多应用程序，这些应用程序包括团队协作软件（例如Confluence）、问题追踪软件（例如JIRA）、集中身份管理系统（例如Crowd）等等。由于这些软件运行在不同的端口上，所以你必须使用Apache或者Nginx来做反向代理。到目前为止，一切都很正常，但是随着你的环境向前推进，你需要在你现有的环境中配置一个内容管理系统（例如Alfresco）。这时候有个问题发生了，这个软件需要一个不同版本的Apache Tomcat，为了满足这个需求，你只能将你现有的软件迁移到另一个版本的Tomcat上，或者找到适合你现有Tomcat的内容管理系统（Alfresco）版本。

对于上述场景，使用Docker就不用做这些事情了。Docker能够确保每个容器都拥有自己的资源，并且和其他容器是隔离的。你可以用不同的容器来运行使用不同堆栈的应用程序。除此之外，如果你想在服务器上直接删除一些应用程序是比较困难的，因为这样可能引发依赖关系冲突。而Docker可以帮你确保应用程序被完全清除，因为不同的应用程序运行在不同的容器上，如果你不在需要一款应用程序，那你可以简单地通过删除容器来删除这个应用程序，并且在你的宿主机操作系统上不会留下任何的临时文件或者配置文件。

除了上述好处，Docker还能确保每个应用程序只使用分配给它的资源（包括CPU、内存和磁盘空间）。一个特殊的软件将不会使用你全部的可用资源，要不然这将导致性能降低，甚至让其他应用程序完全停止工作。

### 安全性

如上所述，Gartner也承认Docker正在快速地发展。从安全角度来看，Docker确保运行在容器中的应用程序和其他容器中的应用程序是完全分隔与隔离的，在通信流量和管理上赋予你完全的控制权。Docker容器不能窥视运行在其他容器中的进程。从体系结构角度来看，每个容器只使用着自己的资源（从进程到网络堆栈）。

作为紧固安全的一种手段，Docker将宿主机操作系统上的敏感挂载点（例如/proc和/sys）作为只读挂载点，并且使用一种写时复制系统来确保容器不能读取其他容器的数据。Docker也限制了宿主机操作系统上的一些系统调用，并且和SELinux与AppArmor一起运行的很好。此外，在Docker Hub上可以使用的Docker镜像都通过数字签名来确保其可靠性。由于Docker容器是隔离的，并且资源是受限制的，所以即使你其中一个应用程序被黑，也不会影响运行在其它Docker容器上的应用程序。

## docker 常用命令

`docker images` 罗列镜像文件

`docker ps` docker 正在运行的容器

docker pull imjcker/jrebel:latest 

docker tag jrebel imjcker/jrebel:latest

docker push imjcker/jrebel:latest

docker拷贝文件

3. 更改现有容器端口

将现有container从新打包为镜像，然后重新部署新容器

```shell
docker stop old-container
docker commit old-container new-container
docker run new-container
```

4. 复制文件

```shell
docker cp foo.txt mycontainer:/foo.txt
docker cp mycontainer:/foo.txt foo.txt
```

**安装**

**Ubuntu 18.04**

1. 卸载老版本

老版本的Docker被叫做 `docker` 或 `docker-engine`. 如果安装了它们，现在就卸载掉：

```
$ sudo apt remove docker docker-engine docker.io
```

如果 `apt` 报出**none of these packages are installed** 这样的信息是正常的。

 `/var/lib/docker/` 目录下的内容包括：images, containers, volumes, networks 等等. 社区版Docker现在叫做 `docker-ce`.

2. 安装社区版Docker-ce
   
   你可以选择多种方式来安装Docker， 这里我们使用`Dockers repositories` 来在线安装。

3. 更新`apt` 包索引
   
   ```shell
   sudo apt update
   ```

4. 安装所需包资源
   
   ```shell
   sudo apt install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg-agent \
       software-properties-common
   ```

5. 添加Docker官方 GPG 
   
   ```
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

6. 设置稳定版本
   
   ```shell
   sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

7. 再次更新apt源
   
   ```shell
   sudo apt update
   ```

8. 安装最新版本的Docker CE 和containerd
   
   ```shell
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

9. 通过测试 `hello-world` 镜像来确认是否安装成功.
   
   ```shell
   sudo docker run hello-world
   ```
   
   这个命令回下载并在一个容器里开启一个镜像，当这个容器运行时，它会打印一些信息并退出。

**CentOS 7**

```shell
# 卸载老版本
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate  docker-engine
# 安装依赖
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
# 设置版本库
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
# 安装docker-ce版本
sudo yum install -y docker-ce docker-ce-cli containerd.io
# 启动docker服务
sudo systemctl start docker
```

**K8s 集群**

**docker install centos**

```shell
docker pull centos:7

sudo docker run --privileged --cap-add SYS_ADMIN -e container=docker -it --network host --name centos7 -p 80:8080  -d  --restart=always centos:7 /usr/sbin/init

docker exec -it centos7 /bin/bash

yum install net-tools

ifconfig -a

yum install openssh-server -y
vi /etc/ssh/sshd_config
systemctl start sshd
systemctl enable sshd
```

docker 创建网络

```shell
# 创建一个名叫imcjker-net的网络
docker network create -d bridge imjcker-net
# 使用指定网路启动一个容器
docker run -d --name xxx --network imjcker-net -p 8080:8080 xxx/xxx:latest
```

## docker 镜像加速

1. CentOS 7 配置
   
   ```shell
   sudo mkdir -p /etc/docker
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://zu9clxov.mirror.aliyuncs.com"]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   
   # "https://zu9clxov.mirror.aliyuncs.com"
   ```

2. MacOS
    右键点击桌面顶栏的 docker 图标，选择 Preferences ，在 Daemon 标签（Docker 17.03 之前版本为 Advanced 标签）下的 Registry mirrors 列表.

3. 111

## docker compose

安装docker compose

```shell
# download 
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# autorization
sudo chmod +x /usr/local/bin/docker-compose
```

## kubernetes

### 安装

### 初始化节点

```shell
kubeadm init \
 --apiserver-advertise-address 0.0.0.0 \
 --apiserver-bind-port 6443 \
 --cert-dir /etc/kubernetes/pki \
 --control-plane-endpoint nas2.imjcker.com \
 --image-repository registry.cn-hangzhou.aliyuncs.com/google_containers \
 --kubernetes-version 1.18.2 \
 --pod-network-cidr 10.10.0.0/16 \
 --service-cidr 10.20.0.0/16 \
 --service-dns-domain cluster.local \
 --upload-certs
```

结果

```shell
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join nas2.imjcker.com:6443 --token crny6e.4m4xl1bjt97vpu7b \
    --discovery-token-ca-cert-hash sha256:3c1638ef886db00df26e938d9479d2c1a9ae3e10db2597a20202fb5b8060d62a \
    --control-plane --certificate-key e386dc9c4ab2349397cdbb543dbcd50a623b1500b7fcb30797b67bf5e12a2380

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join nas2.imjcker.com:6443 --token crny6e.4m4xl1bjt97vpu7b \
    --discovery-token-ca-cert-hash sha256:3c1638ef886db00df26e938d9479d2c1a9ae3e10db2597a20202fb5b8060d62a 
```

```shell
To start administering your cluster from this node, you need to run the following as a regular user:

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

Run 'kubectl get nodes' to see this node join the cluster.
```

## kuboard

```shell
sudo docker run -d \
  --restart=unless-stopped \
  --name=kuboard \
  -p 10080:80/tcp \
  -p 10081:10081/udp \
  -p 10081:10081/tcp \
  -e KUBOARD_ENDPOINT="http://192.168.0.112:10080" \
  -e KUBOARD_AGENT_SERVER_UDP_PORT="10081" \
  -e KUBOARD_AGENT_SERVER_TCP_PORT="10081" \
  -v /root/kuboard-data:/data \
  eipwork/kuboard:v3
```

## docker build image

```shell
docker build -t imjcker/aaa:latest .
```

```shell
docker push imjcker/aaa:latest
```

## 容器迁移

```shell
docker commit -p container_id container_backup
docker save container_backup > container_backup.tar 
docker load < container_bakcup.tar
```

## docker 查看日志

```shell
docker logs --tail 100 frps
```

## 进入docker容器

```shell
docker exec -it xxx /bin/bash
```

## centos 安装docker

```shell
#!/bin/sh
#created by Alan Turing 2019-12-13 for auto install docker on CentOS 7
# run with root

## blue to echo
function blue(){
    echo -e "\033[35m[ $1 ]\033[0m"
}
## green to echo
function green(){
    echo -e "\033[32m[ $1 ]\033[0m"
}
## Error to warning with blink
function bred(){
    echo -e "\033[31m\033[01m\033[05m[ $1 ]\033[0m"
}
## Error to warning with blink
function byellow(){
    echo -e "\033[33m\033[01m\033[05m[ $1 ]\033[0m"
}
## Error
function red(){
    echo -e "\033[31m\033[01m[ $1 ]\033[0m"
}
## warning
function yellow(){
    echo -e "\033[33m\033[01m[ $1 ]\033[0m"
}

# -------------------------------------------------------------------------
green "卸载老版本"
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate  docker-engine
green "安装依赖"
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
green "设置版本库"
#sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
green "安装docker-ce版本"
sudo yum install -y docker-ce docker-ce-cli containerd.io
green "启动docker服务"
sudo systemctl start docker
green "设置开机启动"
sudo systemctl enable docker
```

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