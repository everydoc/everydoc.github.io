---
title: Windows All in One
date: 2021-11-05 23:32:59.146
updated: 2021-11-05 23:32:59.146
url: https://halo.imjcker.com/archives/windowsallinone
categories: 
tags: windows
---


Windows系统上使用记录。

**Windows** 内外网同时使用配置路由

> > 在命令提示符里面输入：route add -p 10.173.0.0 mask 255.255.0.0 192.168.1.1
> > 这样设置就是把有线连接需要网络的时候就会连接到服务器：10.173.0.0（无线wifi的网段
>
> route add -p 172.20.10.1 mask 255.255.0.0 192.168.2.1
>
> 2、在命令提示符里输入：route add 172.18.0.0 mask 255.255.0.0 172.18.100.254
> 这里给大家解释一下，这套命令的作用是把所有访问内网172.18.xxx.xxx的请求转发给内网网关172.18.100.254。
> 如果你们的内网还有其它网段的话就照上面的格式依次添加即可。比如：你们公司的内网还有这样的地址172.16.1.25，
> 你就可以这样写：route add 172.16.0.0 mask 255.255.0.0 172.18.100.254

**Windows**查杀进程

第一步，根据端口号查找对应的进程号

```powershell
netstat -ano | findstr 80 //列出进程极其占用的端口，且包含 80
```

第二步，据进程号寻找进程名称

```powershell
tasklist | findstr 2000  
```

第三步，杀进程

```powershell
taskkill -PID <进程号> -F //强制关闭某个进程  
```

```powershell
@echo off
:begin

rem: 设置编码
@chcp 65001
echo 开始执行脚本

rem: 执行jps命令，查找Java进程
jps

rem: 输入变量
set pid=
set /p pid=请输入要关闭的进程号,或回车跳过此步骤： 
if "%pid%"=="" (echo 不执行查杀进程，) else taskkill -pid %pid% -f
if %errorlevel% == 0 echo succeed. else echo failed.

set service=
set /p service=请输入要运行的Java程序地址,或回车跳过此步骤：
if "%service"=="" goto begin
if exist %service% (java -jar %service%) else (没有找到指定地址：%service%)
rem: if %errorlevel% gt 0 echo 执行程序报错。。。

:ddd
pause
exit
```