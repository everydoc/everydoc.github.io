---
title: Linux All in One
date: 2021-11-05 23:32:13.013
updated: 2022-02-17 14:10:48.359
url: https://halo.imjcker.com/archives/linuxallinone
categories: 默认分类
tags: ssh | linux | xshell
---



常用Linux命令使用记录
![linux](https://imjcker.com:1990/upload/2022/01/linux-81f8be78b70b4edeba4dc6d3bc96cbf4.png)
## 常用命令

**实用命令**

1. fuser

   > 查看文件被谁占用。
   >
   > ```shell
   > fuser -u .linux.md.swp
   > ```

2. id

   > 查看当前用户、组 id。

3. lsof

4. find / -iname fileName

5. / 在vi中查找 n下一个 N上一个

查看打开的文件列表。

> An  open  file  may  be  a  regular  file,  a directory, a block special file, a character special file, an executing text reference, a library, a stream or a network file (Internet socket, NFS file or UNIX domain socket.)  A specific file or all the files in a file system may be selected by path.

1. 查看网络相关的文件占用

   ```shell
   lsof -i
   ```

2. 查看端口占用

   ```shell
   lsof -i tcp:5037
   
   netstat -tunlp | grep 8000
   ```

3. 查看某个文件被谁占用

   ```shell
   lsof .linux.md.swp
   ```

4. 查看某个用户占用的文件信息

   ```shell
   lsof -u mazhuang
   # -u 后面可以跟 uid 或 login name。
   ```

5. 查看某个程序占用的文件信息

   ```shell
   lsof -c Vim
   # 注意程序名区分大小写。
   ```


6. centos 安装卸载rpm包

   ```shell
   # 安装
   rpm -ivh xxx.rpm
   # 卸载
   rpm -e xxx
   ```

7. 清理内存

   ```shell
   echo 1 > /proc/sys/vm/drop_caches
   ```

   

记录工作中需要用到一些常用命令、使用技巧、开发配置。

**基础命令**

| 命令及参数                                                   | 用途及说明                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| df -h                                                        | 查看硬盘使用情况                                             |
| top                                                          | 实时系统参数使用情况（Windows的任务管理器）                  |
| passwd username                                              | 更改用户密码，不输入username时默认修改root用户密码           |
| mv 原文件 目标文件                                           | 移动文件                                                     |
| cp 原文件 目标文件                                           | 复制文件                                                     |
| chmod -R 777 文件名                                          | 更改文件或目录权限``` -R``` 表示连带更改下级目录权限，```777```是权限级别，这个级别有很多 |
| ```nc -l port```                                             | 在指定端口开启一个```tcp```服务                              |
| ``` nc ip port```                                            | 连接指定IP及端口的```tcp```服务                              |
| ```python -m SimpleHTTPServer 8000```                        | 利用python开启一个临时的http服务，在后面追加```&```表示后台启动 |
| ```scp -r -P port 源文件 user@ip:目标文件``` ``` scp -r -P port user@ip:源文件 目标文件``` | 利用ssh从本地传递文件到服务器，```-r```传输文件夹，单个文件不需要该参数，```-P```大写。后者是从服务器下载文件到本地指定目录 |
| swapon -s                                                    | 查看交换空间设置情况                                         |
| free -m                                                      | 查看内存以及swap使用情况                                     |
| tar -zxvf filename.tar -C /specific-dir                      | 解包到指定的目录                                             |
| tar -zcvf filename.tar filename                              | 打包                                                         |





```shell
# 创建用户
useradd imjcker
passwd imjcker


```

**发行版**

1. Ubuntu

The position could only be either *Left* or *Bottom*

script for Bottom:

> ```
> gsettings set com.canonical.Unity.Launcher launcher-position Bottom
> 
> ```

script for Left:

> ```
> gsettings set com.canonical.Unity.Launcher launcher-position Left
> 
> ```

Ubuntu禁止熄屏

```
sudo gedit /etc/systemd/logind.conf

```

打开文件后修改下面这行：

```
#HandleLidSwitch=suspend

```

改成这样：

```
HandleLidSwitch=ignore

```

保存文件，重启 Login Manager 服务：

```
sudo systemctl restart systemd-logind

```

工作完成！



**更改**Linux系统编码
$ vim /etc/locale.conf
LANG="zh_CN.UTF-8"

Step 2
$ vi ~/.bashrc

# 追加
export LANG='UTF-8'
export LC_ALL='zh_CN.UTF-8'
export LC_CTYPE='zh_CN.UTF-8'

$ source ~/.bashrc


```shell
vi /etc/sysconfig/i18n
#LANG="zh_CN.GBK" 修改为LANG="zh_CN.UTF-8"保存退出
#是更改生效
source /etc/sysconfig/i18n
#检查编码：
locale

```

### 开关防火墙

1. CentOS

```shell
# 关闭防火墙
systemctl stop firewalld
# 永久关闭
systemctl disable firewall
# 开放端口方法1
firewall-cmd --add-port=6379/tcp --zone=public --permanent
# 对指定IP开放端口
firewall-cmd --add-rich-rule="rule family="ipv4" source address="192.168.142.166" port protocol="tcp" port="6379" accept" --permanent

firewall-cmd --zone=public --list-ports
firewall-cmd --reload
# 如果添加端口报错 INVALID_PORT，可以直接去对应目录添加配置，然后reload。
vi /etc/firewalld/zones/public.xml

# 开放端口方法2
iptables

如要开放80，22，8080 端口，输入以下命令即可
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
1
2
3
然后保存：
/etc/rc.d/init.d/iptables save
查看打开的端口：
/etc/init.d/iptables status
关闭防火墙 
1） 永久性生效，重启后不会复原
开启： chkconfig iptables on
关闭： chkconfig iptables off
2） 即时生效，重启后复原
开启： service iptables start
关闭： service iptables stop

```

## 性能监控





## VI

Linux vi 操作，这是个人记录，不是适合零基础小伙伴学习使用。



dd 删除一行

d$ 删除以当前字符开始的一行字符

ndd 删除以当前行开始的n行

dw 删除以当前字符开始的一个字

ndw 删除以当前字符开始的n个字

D 与d$同义

d) 删除到下一句的开始

d} 删除到下一段的开始

d回车 删除2行



**以单词为单位**

| 功能                                   | 按键 |
| :------------------------------------- | :--- |
| 前一个单词尾                           | `ge` |
| 后一个单词首                           | `w`  |
| 本单词首（已在本词首则跳到前一单词首） | `b`  |
| 本单词尾（已在本词尾则跳到后一单词尾） | `e`  |

**以屏幕为单位**

| 功能                     | 按键     |
| :----------------------- | :------- |
| 向下翻页                 | `CTRL-f` |
| 向上翻页                 | `CTRL-b` |
| 向下翻半页               | `CTRL-d` |
| 向上翻半页               | `CTRL-u` |
| 向上一行                 | `CTRL-y` |
| 向下一行                 | `CTRL-e` |
| 光标移到屏幕上方         | `H`      |
| 光标移到屏幕中间         | `M`      |
| 光标移到屏幕下方         | `L`      |
| 光标所在位置移到屏幕上方 | `zt`     |
| 光标所在位置移到屏幕中间 | `zz`     |
| 光标所在位置移到屏幕下方 | `zb`     |

**行号**

| 功能          | 按键                        |
| :------------ | :-------------------------- |
| 跳到第 num 行 | `:num` 或 `numG` 或 `numgg` |

**文件**

| 功能       | 按键 |
| :--------- | :--- |
| 跳到文件头 | `gg` |
| 跳到文件尾 | `G`  |

**编辑**

**复制**

| 功能             | 按键  |
| :--------------- | :---- |
| 复制光标所在单词 | `yiw` |
| 复制光标所在行   | `yy`  |

**粘贴**

| 功能           | 按键 |
| :------------- | :--- |
| 在光标之后粘贴 | `p`  |
| 在光标之前粘贴 | `P`  |

**剪切**

| 功能           | 按键 |
| :------------- | :--- |
| 剪切选中区域   | `d`  |
| 剪切光标所在行 | `dd` |

**替换**

| 功能                              | 按键                |
| :-------------------------------- | :------------------ |
| 将全文中的 str1 替换为 str2       | `:%s/str1/str2/g`   |
| 将 1 到 5 行中的 str1 替换为 str2 | `:1,5s/str1/str2/g` |

**大小写**

| 功能                 | 按键  |
| :------------------- | :---- |
| 将选中内容大小写互换 | `~`   |
| 将选中内容全转为小写 | `gu`  |
| 将选中内容全转为大写 | `gU`  |
| 将当前行变成小写     | `guu` |
| 将当前行变成大写     | `gUU` |

**选择**

| 功能                 | 按键                |
| :------------------- | :------------------ |
| 选中上一次选择的区域 | `gv`                |
| 选中括号内区域       | `vi{`、`vi[`、`vi(` |

**搜索**

| 功能                 | 按键   |
| :------------------- | :----- |
| 向下查找字符串       | `/str` |
| 向上查找字符串       | `?str` |
| 查找下一个           | `n`    |
| 查找上一个           | `N`    |
| 向下查找光标所在单词 | `*`    |
| 向下查找光标所在单词 | `#`    |

**正则表达式**

| 功能           | 按键                |
| :------------- | :------------------ |
| 匹配单词左边界 | `\<`                |
| 匹配单词右边界 | `\>`                |
| 去重           | `:g/^\(.*\)$\n\1/d` |

**常用**

| 功能                    | 按键      |
| :---------------------- | :-------- |
| 删除空行                | `:g/^$/d` |
| 撤销/UNDO               | `u`       |
| 重做/REDO               | `C-r`     |
| 统计行/单词/字符/字节数 | `g C-g`   |

**全局**

| 功能         | 按键 |
| :----------- | :--- |
| 退出         | `:q` |
| 强制执行     | `!`  |
| 执行外部命令 | `:!` |

**文件操作**

| 功能               | 按键          |
| :----------------- | :------------ |
| 打开               | `:e`          |
| 打开文件对话框     | `:bro e`      |
| 保存               | `:w`          |
| 另存为对话框       | `:bro w`      |
| 查看历史文件列表   | `:ol`         |
| 查看并打开历史文件 | `:bro ol`     |
| 重命名当前文件     | `:f filename` |

**vimdiff**

| 功能                   | 按键          |
| :--------------------- | :------------ |
| 移动到上一个不同处     | `[c`          |
| 移动到下一个不同处     | `]c`          |
| 该差异点使用当前文件的 | `dp`          |
| 该差异点使用其它文件的 | `do`          |
| 手动刷新重新比较       | `:diffupdate` |

**Buffer**

| 功能                               | 按键     |
| :--------------------------------- | :------- |
| 查看 Buffer 列表                   | `:ls`    |
| 转到 Buffer 列表中的下一个 Buffer  | `:bn`    |
| 转到 Buffer 列表中的上一个 Buffer  | `:bp`    |
| 转到 Buffer 列表中的 num 号 Buffer | `:bnum`  |
| 你之前待过的一个 Buffer            | `:b#`    |
| 从 Buffer 列表中删除 num 号 Buffer | `:bdnum` |

**组合命令**

可以使用 `|` 来组合命令，比如 `cmd1 | cmd2`。

**代码**

| 功能                    | 按键                           |
| :---------------------- | :----------------------------- |
| 格式化代码              | `gg=G`                         |
| 去除 1-20 行首的行号    | `:1,20s/^\\s\*[0-9]\*\\s\*//g` |
| 展开全部折叠            | `zR`                           |
| 展开当前层级折叠        | `zr`                           |
| 全部折叠                | `zM`                           |
| 当前层级折叠            | `zm`                           |
| 切换折叠/展开           | `za`                           |
| 递归折叠/展开当前大区块 | `zA`                           |
| 折叠当前区块            | `zc`                           |
| 递归折叠当前大区块      | `zC`                           |
| 展开当前区块            | `zo`                           |
| 递归展开当前大区块      | `zO`                           |
| 格式化 json 数据        | `:%!python -m json.tool`       |
| 缩进当前行              | `>>`                           |
| 反缩进当前行            | `<<`                           |

**插件**

1. CtrlP

基础按键 `C-p`

| 功能                               | 按键      |
| :--------------------------------- | :-------- |
| 刷新列表                           | `F5`      |
| 切换文件/缓冲区/MRU                | `C-f/b`   |
| 切换全路径搜索/文件名搜索          | `C-d`     |
| 切换正则表达式模式                 | `C-r`     |
| 上/下一个选项                      | `C-k/j`   |
| 在新标签/垂直分割/水平分割打开文件 | `C-t/v/x` |
| 历史选择记录的上/下一条            | `C-p/n`   |
| 创建文件和它的父路径               | `C-y`     |
| 标记并打开多个文件                 | `C-z C-o` |
| 退出 CtrlP                         | `C-c`     |

2. LeaderF

| 功能                               | 按键                   |
| :--------------------------------- | :--------------------- |
| 打开文件                           | `Leader-f`             |
| 打开缓冲区                         | `Leader-b`             |
| 打开 MRU                           | `Leader-m`（自定义的） |
| 退出                               | `C-c`                  |
| 切换模糊查找和正则查找             | `C-r`                  |
| 粘贴                               | `C-v`                  |
| 清空输入                           | `C-u`                  |
| 上/下一个选项                      | `C-k/j`                |
| 在新标签/垂直分割/水平分割打开文件 | `C-t/]/v`              |
| 刷新列表                           | `F5`                   |

3. vim-table-mode

| 功能           | 按键         |
| :------------- | :----------- |
| 删除列         | `Leader-tdc` |
| 删除行         | `Leader-tdd` |
| 重新格式化表格 | `Leader-tr`  |



## ssh

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



## shell

shell脚本能解放生产力，记录shell脚本使用情况

## shell 编程


#### ssh 密钥对使用

在开发和测试的时有很多场景都要用到ssh连接远程服务器，每次都输入密码让人真的很头痛，那么有说明办法可以避免每次都输入密码呢？答案就是：ssh密钥对

```shell
# 生成密钥对
ssh-keygen -t rsa -b 4096 -C "this is a ssh key demo" # -t指加密方式，-b指加密长度
# 一直回车默认直到生成密钥对在默认位置，一般为当前用户的根目录下的.ssh目录下
cd ~/.ssh
# 私钥：id_rsa ， 公钥：id_rsa.pub
# 配置私钥服务
# 检查私钥服务状况
eval "$(ssh-agent -s)"
# 打印pid说明正常，开始配置私钥
ssh-add ~/.ssh/id_rsa
# 查看添加状况
ssh-add -l


```


#### 脚本入参

```shell
$# 入参个数
$1 第一个入参，以此类推
```



#### telnet

telnet: Unable to connect to remote host: No route to host

执行： 

```shell
iptables -F
```



#### xsync

```shell
#!/bin/bash

pcount=$#
if((pcount==0)); then
echo no args;
exit;
fi

p1=$1
fname=`basename $p1`
echo fname=$fname

pdir=`cd -P $(dirname $p1); pwd`
echo pdir=$pdir

user=`whoami`

for host in hadoop-m hadoop-s1 hadoop-s2 hadoop-s3
do
        echo -------------------hadoop$host --------------
        rsync -rvl $pdir/$fname $user@$host:$pdir
done
```

## 基本颜色函数

```shell script
#!/usr/bin/env bash

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

```

### getPid

```shell script
# get pid

getPID(){
	pid=$(ps -ef | grep "$1" |grep -v grep | awk '{print $2}')
	echo "$pid"
}

getPID "$1"
```

### 删除乱码文件

```shell 
ls -i

find ./ -inum 3538983 -exec rm -rf {} \;
```



## 有用的脚本

万能jar包启动命令

```shell
#!/usr/bin/env bash

VM_OPTS="-Xms4096m -Xmx4096 -Xss32M"
SPB_OPTS="--spring.profile.active=dev"
APP_LOCATION="/root/ddns/ddns-0.0.1.jar"
APP_NAME="ddns-0.0.1.jar"
PID_CMD="ps -ef|grep $APP_NAME | grep -v grep | awk '{print \$2}'"

start(){
  PID=$(eval $PID_CMD)
  if [[ -n $PID ]]; then
    echo "$APP_NAME is already running, PID is $PID"
  else
    nohup java "$VM_OPTS" -jar $APP_LOCATION $SPB_OPTS > /dev/null 2>&1 &
    echo "nohup java $VM_OPTS -jar $APP_LOCATION $SPB_OPTS > /dev/null 2>&1 &"
    PID=$(eval $PID_CMD)
    if [[ -n $PID ]]; then
      echo "start $APP_NAME succeed, PID is $PID"
    else
      echo "start $APP_NAME failed."
    fi
  fi
}

stop(){
  PID=$(eval $PID_CMD)
  if [[ -n $PID ]]; then
    kill -15 $PID
    sleep 3
    PID=$(eval $PID_CMD)
    if [[ -n $PID ]]; then
      echo "stop $APP_NAME failed by using kill -15 $PID, begin to kill -9 $PID"
      kill -9 $PID
      sleep 2
      echo "stop $APP_NAME succeed"
    else
      echo "stop $APP_NAME succeed"
    fi
  fi
}

restart(){
  stop
  start
}

status(){
  PID=$(eval $PID_CMD)
  if [[ -n $PID ]]; then
    echo "$APP_NAME is running, PID is $PID"
  else
    echo "$APP_NAME is not running"
  fi
}

info(){
  echo "APP_LOCATION: $APP_LOCATION"
  echo "APP_NAME: $APP_NAME"
  echo "VM_OPTS: $VM_OPTS"
  echo "SPB_OPTS: $SPB_OPTS"
}

help(){
  echo "available commands: start, stop, restart, status, info, help"
}

case $1 in
start)
  start
  ;;
stop)
  stop
  ;;
restart)
  restart
  ;;
status)
  status
  ;;
info)
  info
  ;;
help)
  help
  ;;
*)
  help
  ;;
esac
exit $?
```



cloudflare DDNS

```shell
#!/bin/bash

current_ip=$(curl ip.sb)
echo "current local ip: $current_ip" > /var/log/ddns.log

previous_ip=$(ping imjcker.com -c 1 | sed -e '2,$d' -e 's/^.*imjcker.com (//g' | sed 's/).*$//g')
echo "previous ip: $previous_ip" >> /var/log/ddns.log

if [[ $current_ip == "$previous_ip" ]]; then
    echo 'server ip is not changed, do nothing.' >> /var/log/ddns.log
    exit 0
fi

# get dns id
getIdResult=$(curl -X GET "https://api.cloudflare.com/client/v4/zones/96339b32f2b65f607c5bb90344e989a4/dns_records?type=A&name=imjcker.com" \
     -H "X-auth-Email: helloalanturing@icloud.com" \
     -H "X-auth-Key: 639164cf0ab9e7652ad48ce90c061e787cfa8" \
     -H "Content-Type: application/json")
id=$(echo "$getIdResult" | sed 's/^.*"id":"//g' | sed 's/",".*$//g')
echo "dns id is: $id" >> /var/log/ddns.log

# update dns
echo "update dns"
curl -X PUT "https://api.cloudflare.com/client/v4/zones/96339b32f2b65f607c5bb90344e989a4/dns_records/$id" \
     -H "X-auth-Email: helloalanturing@icloud.com" \
     -H "X-auth-Key: 639164cf0ab9e7652ad48ce90c061e787cfa8" \
     -H "Content-Type: application/json" \
     -d "{\"type\":\"A\",\"name\":\"imjcker.com\",\"content\":\"$current_ip\",\"ttl\":2,\"proxied\":false}"

getCurrentIP=$(curl -X GET "https://api.cloudflare.com/client/v4/zones/96339b32f2b65f607c5bb90344e989a4/dns_records?type=A&name=imjcker.com" \
     -H "X-auth-Email: helloalanturing@icloud.com" \
     -H "X-auth-Key: 639164cf0ab9e7652ad48ce90c061e787cfa8" \
     -H "Content-Type: application/json")
ip=$(echo "$getCurrentIP" | sed 's/^.*"content":"//g' | sed 's/",".*$//g')
# write log
echo "current remote ip: $ip" >> /var/log/ddns.log


```







![arthas](https://imjcker.com:1990/upload/2022/01/arthas-dca76868c2834330a5c5c644bc5f8a7d.png)

Arthas是Alibaba开源的Java诊断工具，深受开发者喜爱。在线排查问题，无需重启；动态跟踪Java代码；实时监控JVM状态。支持JDK 6+，支持Linux/Mac/Windows，采用命令行交互模式，同时提供丰富的 `Tab` 自动补全功能，进一步方便进行问题的定位和诊断。

下面记录了在centos服务器使用的情况。



# Arthas



## 

从GitHub下载最新版全量包`arthas-3.2.0-bin.zip`

https://github.com/alibaba/arthas/releases

解压压缩包. 

```shell
unzip arthas-3.2.0-bin.zip
```

![1](/assets/2020/arthas-1.png)



进入arthas目录. 

```shell
cd arthas
```

![2](/assets/2020/arthas-2.png)

启动. 

```shell
./as.sh
```

![3](/assets/2020/arthas-3.png)



在系统中选择已经启动的Java进程对应的编号，出现下图情况表示启动成功。

![4](/assets/2020/arthas-4.png)



## 命令使用

### 基础命令

- help——查看命令帮助信息
- cls——清空当前屏幕区域
- session——查看当前会话的信息
- reset——重置增强类，将被 Arthas 增强过的类全部还原，Arthas 服务端关闭时会重置所有增强过的类
- version——输出当前目标 Java 进程所加载的 Arthas 版本号
- history——打印命令历史
- quit——退出当前 Arthas 客户端，其他 Arthas 客户端不受影响
- stop——关闭 Arthas 服务端，所有 Arthas 客户端全部退出
- keymap——Arthas快捷键列表及自定义快捷键



### dashboard

打印当前系统的实时数据信息面板包括：当前jvm的线程情况，jvm的内存分配、使用量、GC情况，以及当前运行环境情况。

```shell
dashboard
```

![5](/assets/2020/arthas-5.png)



### monitor

`monitor`是一个方法执行的监控，对匹配的类的方法进行监控。

```shell
# 监控指定类的指定方法 周期60秒，默认周期120秒；类名和方法名可以使用ognl表达式
monitor -c 60 com.imjcker.spring.cloud.service.wechat.schedule.CallApiTask CallApi
```

![6](/assets/2020/arthas-6.png)

| 监控项    | 说明                       |
| --------- | -------------------------- |
| timestamp | 时间戳                     |
| class     | Java类                     |
| method    | 方法（构造方法、普通方法） |
| total     | 调用次数                   |
| success   | 成功次数                   |
| fail      | 失败次数                   |
| rt        | 平均RT                     |
| fail-rate | 失败率                     |

### trace

`trace`追踪方法内部调用情况，渲染和统计整个调用链路上的所有性能开销和追踪调用链路，输出调用链路和耗时情况。

```shell
# 追踪 ManagerFeign类的 sayHi 方法， 次数 100， 默认200次
trace com.imjcker.spring.cloud.service.wechat.feign.ManagerFeign sayHi -n 100
```

![7](/assets/2020/arthas-7.png)

```shell
# 过滤 CallApiTask 类的 CallApi 方法大于1ms的调用情况。
trace com.imjcker.spring.cloud.service.wechat.schedule.CallApiTask CallApi '#cost > 1'
```

![8](/assets/2020/arthas-7.png)



### stack

`stack`输出当前方法被调用的调用路径。很多时候我们都知道一个方法被执行，但这个方法被执行的路径非常多，或者你根本就不知道这个方法是从那里被执行了，此时你需要的是 stack 命令。

```shell
# 追踪 ManagerFeign lei 类的 sayHi 方法的调用路径
stack com.imjcker.spring.cloud.service.wechat.feign.ManagerFeign sayHi
```

![9](/assets/2020/arthas-9.png)



### thread

查看当前 JVM 的线程堆栈信息

![10](/assets/2020/arthas-10.png)





# ApiSix部署使用

## 1.安装部署

1. 安装epel

   ```shell
   rpm -ivh epel-release-latest-7.noarch.rpm
   ```

   

2. 安装etcd

   ```shell
   tar -xvf etcd-v3.4.13-linux-amd64.tar.gz
   cp -a etcd-v3.4.13-linux-amd64/etcd etcd-v3.4.13-linux-amd64/etcdctl /usr/bin/
   nohup etcd >/var/log/etcd.log 2>&1 &
   
   # 开机启动
   
   ```

   

3. 安装openresty

   ```shell
   rpm -ivh openresty-*.rpm
   ```

   

4. 安装apisix

   ```shell
   yum -y install apisix-2.1-0.el7.noarch.rpm
   ```

   

5. 启动

   ```shell
   apisix version
   apisix start
   ```

   

## 2.安装dashboard

```shell
$ sudo yum install -y https://github.com/apache/apisix-dashboard/releases/download/v2.6/apisix-dashboard-2.6-0.x86_64.rpm

$ sudo nohup manager-api -p /usr/local/apisix/dashboard/ &
```





## 3.配置注册中心

对接Eureka





## 科学上网


科学上网之安装shadowsocks到centos7。

```shell
# 下载 pip
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
# 安装 pip
python get-pip.py
# 安装shadowsocks
pip install shadowsocks
# 启动shadowsocks
ssserver -p 13579 -k ss.imjcker.com -m chacha20 --user nobody -d start

```

### service 配置

```shell
[Unit]
Description=Shadowsocks Server Daemon

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /root/shadowsocks/ssconfig.json

[Install]
WantedBy=multi-user.target
```







## docker 快速安装

```shell
docker run -d --name ssserver -p 13579:8388 -p 13579:8388/udp --restart always -e PASSWORD=ss.imjcker.com -e METHOD=chacha20 shadowsocks/shadowsocks-libev:latest
```





# https证书

# SSL教程



来源：https://cnodejs.org/topic/5be29f7c21d75b74609f4fbf



```shell
./certbot-auto certonly  -d *.imjcker.com --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```





```shell
[root@imjcker ~]# certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: imjcker.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 
Requesting a certificate for imjcker.com

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/imjcker.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/imjcker.com/privkey.pem
This certificate expires on 2021-12-31.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for imjcker.com to /etc/nginx/nginx.conf
Congratulations! You have successfully enabled HTTPS on https://imjcker.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

# 基于非80/443端口的https证书申请

由于某些原因，在不使用云服务器的时候，由于运营商的封锁，80/443等端口被封锁而无法使用，故此在写下此篇博客，来记录 自己如何通过使用非80/443等常规端口在letsencrypt上申请https证书。





## 资料来源

https://letsencrypt.org/zh-cn/docs/challenge-types/#dns-01-%E9%AA%8C%E8%AF%81

#acme.sh 申请证书

## 基于腾讯的dnspod.cn

*注意⚠️：犹豫腾讯升级接口到3.0 要区分开dnspod和两种调用接口的密钥区别*

犹豫acme.sh没有升级相关脚本，现在还是使用老接口。

DP_Id DP_Key 使用的是dnspod token，而不是腾讯云API密钥

## 安装

```shell
curl https://get.acme.sh | sh -s email=my@example.com
```

## 配置密钥

`DP_Id`
`DP_Key`

## 发布

```shell
 acme.sh --issue --dns dns_dp -d imjcker.com -d '*.imjcker.com'
```





# 通用服务器配置

# 服务器配置

## 群晖虚拟机设置

### 开启IPv6

通过克隆基础版本centos虚拟机，导致系统无法获取到IPv6，这是因为网卡的uuid一致导致的，需要到`/etc/sysconfig/network-script/ifcfg-xxx`修改UUID。



### 装载工具

在虚拟机的编辑-->其它，其它ISO文件选择synology-vmm-guest-tool,并在虚拟机执行，`yum -y install qemu-guest-agent`。



### 安装动态域名解析DDNS

下载GitHub的ddns-go，并根据其说明安装配置

如果有docker，执行`docker run -d --name ddns --restart=always --net=host -v /etc/ddns-go:/root jeessy/ddns-go:latest`

#### 配置

> 300443 0cac346013efcabf7b2a096364963136





### 防火墙



1. 开通指定端口：`firewall-cmd --add-port=9876/tcp --zone=public --permanent`
2. 刷新防火墙：`firewall-cmd --reload`
3. 查看开通端口：`firewall-cmd --zone=public --list-ports`
4. 不安全策略，关闭防火墙：`systemctl disable firewalld`



## 一般vps配置

```
# 设置快捷键
cat >> ~/.bashrc <<EOF
alias cc='sync; echo 3 > /proc/sys/vm/drop_caches'
alias fm='free -m'
# Dnspod token
export DP_Id="300443"
export DP_Key="0cac346013efcabf7b2a096364963136"
EOF

soruce .bashrc

# 安装docker
curl https://get.docker.com | sh
systemctl enable docker && systemctl start docker

# 安装nginx
yum -y install nginx
systemctl enable nginx && systemctl start nginx

# 申请ssl证书
curl https://get.acme.sh | sh -s email=helloalanturing@icloud.com
acme.sh --issue --dns dns_dp -d imjcker.com -d *.imjcker.com
# 无法获取脚本的话使用git克隆
git clone https://github.com/acmesh-official/acme.sh.git
cd ./acme.sh
./acme.sh --install -m helloalanturing@icloud.com

# 安装v2ray
curl https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh -o v2ray.sh && chmod +x v2ray.sh && sh v2ray.sh
# v2raya 
docker run -d \
  --restart=always \
  --privileged \
  --network=host \
  --name v2raya \
  -e V2RAYA_LOG_FILE=/tmp/v2raya.log \
  -v /lib/modules:/lib/modules:ro \
  -v /etc/resolv.conf:/etc/resolv.conf \
  -v /etc/v2raya:/etc/v2raya \
  mzz2017/v2raya

# 启动frps

```



# 卸载阿里云监控软件



## 最新云服务器已经不支持这种方式了，需手动去云平台卸载。



```shell
curl http://update.aegis.aliyun.com/download/uninstall.sh | sh
curl http://update.aegis.aliyun.com/download/quartz_uninstall.sh | sh
```



## 删除服务

```shell
pkill aliyun-service
rm -fr /etc/init.d/agentwatch /usr/sbin/aliyun-service
rm -rf /usr/local/aegis*
```



# centos 7 install pip

您可以按照以下步骤在 CentOS 7 上安装 pip：

更新您的系统：

```shell
sudo yum update
```

安装必要的软件包：

```shell
sudo yum install epel-release
sudo yum install python-pip
```

验证 pip 是否安装成功：

```shell
pip --version
```

如果一切顺利，您应该能够看到安装的 pip 版本号。

如果出现错误，您可能需要安装一些其他的软件包或更新您的系统。

