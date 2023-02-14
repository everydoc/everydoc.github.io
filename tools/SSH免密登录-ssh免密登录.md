---
title: SSH免密登录
date: 2021-11-05 23:32:10.821
updated: 2021-11-05 23:32:10.821
url: https://halo.imjcker.com/archives/ssh免密登录
categories: 
tags: ssh
---


在开发和测试的时有很多场景都要用到ssh连接远程服务器，每次都输入密码让人真的很头痛，那么有什么办法可以避免每次都输入密码呢？答案就是：ssh密钥对
![ssh](/assets/2020/ssh.png)

**下面演示图片的场景**  
如上图所示，你要通过自己的电脑💻连接很多的服务器，为了节省工作，或者安全问题，我们使用ssh密钥对来完成代替密码输入连接远程服务器的工作。

## 生成密钥
打开自己电脑（这里以Mac为例）在控制台输入并回车
```shell
ssh-keygen -t rsa -b 4096 -C "this is a ssh key demo" 
# -t指加密方式默认是rsa，-b指加密长度默认是2048，-C是说明方便查看
```
然后会提示你输入保存的文件名，默认为 id_rsa 和 id_rsa.pub，这里我们输入的`～/.ssh/aaa`。
接下来提示输入密码，这个密码是指采用密钥对连接远程服务器时使用的密码，不是登录远程服务器的密码，这里为了方便我们直接回车表示不要密码。
最后会提示生成了`aaa`和`aaa.pub`两个文件，这就是我们登录远程服务器的密钥对了，私钥：aaa ， 公钥：aaa.pub
![ssh-1](/assets/2020/ssh-1.png)

## 配置密钥
## 私钥配置（本地）
生成密钥时创建的文件保存在了当前用户下的.ssh目录下。
进入`~/.ssh`目录。
```shell 
cd ~/.ssh 
# 查看目录下的文件列表
ls -la
# 添加公钥到ssh代理
ssh-add aaa
# 如果出现Could not open a connection to your authentication agent.类似问题，
# 执行 exec ssh-agent bash 再重试。

# 打印公钥的内容
cat aaa.pub
# 将打印的内容复制下来，一会儿使用
```
![ssh-1](/assets/2020/ssh-2.png)
### 公钥配置（服务器）
接下来我们开始配置远程服务器的公钥。
首先要通过ssh输入密码的方式连接到远程服务器进行公钥配置。
进入服务器的`~/.ssh`目录。
```shell 
cd ~/.ssh
# 查看目录下的文件列表
ls -la
# 编辑 authorized_keys 文件，没有会自动创建
vi authorized_keys
# 将刚才复制的公钥 `aaa.pub` 的内容追加到 authorized_keys 最后并保存。
```
在其他服务器重复**公钥配置**操作，为每台服务器添加公钥，至此，配置完成。

## 测试连接
回到自己电脑上，再次ssh连接刚才配置的服务器，你会发现不用输入密码就可以直接连接到远程的服务器上了。