---
title: Arthas使用教程
date: 2022-01-18 19:22:01.651
updated: 2022-01-18 19:22:01.651
url: https://halo.imjcker.com/archives/arthas-shi-yong-jiao-cheng
categories: 测试
tags: arthas | test | 测试
---

![arthas](https://imjcker.com:1990/upload/2022/01/arthas-dca76868c2834330a5c5c644bc5f8a7d.png)

Arthas是Alibaba开源的Java诊断工具，深受开发者喜爱。在线排查问题，无需重启；动态跟踪Java代码；实时监控JVM状态。支持JDK 6+，支持Linux/Mac/Windows，采用命令行交互模式，同时提供丰富的 `Tab` 自动补全功能，进一步方便进行问题的定位和诊断。

下面记录了在centos服务器使用的情况。

## 下载安装

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