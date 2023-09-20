---
title: VirtualBox
date: 2021-11-05 23:32:59.139
updated: 2022-01-19 01:29:01.406
url: https://halo.imjcker.com/archives/virtualbox
categories: 
tags: vm
---

## 在VirtualBox中安装和克隆CentOS

***前言***

​	最近开始学习ActiveMQ+ZooKeeper的集群，需要建立至少3个服务器，考虑到本人只有2个VPS，遂选择在自己的Mac上安装VirtualBox搭建虚拟机。以下是搭建环境中遇到的一些问题和总结。

1. 快速搭建3个虚拟机

   事实上在安装好一个虚拟机后，便可通过**克隆**的形式快速创建多个虚拟机。

2. 网络问题

3. 原始虚拟机网络

   考虑资源问题，下载的是CentOS Minimal版本的镜像文件。安装好后马上测试了一下联网情况  

```
ping jcker.org
```

发现无法ping通  

***解决方案***

```
vi /etc/sysconfig/network-scripts/ifcfg-xxx
#xxx视具体情况而定，可能是eth0什么的。
```

将**ONBOOT**参数的值改为__yes__ ，然后重启网络服务：

```
service network restart

```

再次尝试ping外网，就能够ping通了。

4. 克隆虚拟机网络

   克隆出来的虚拟机无法联网，这是因为mac地址虽然在克隆时选择了全部重新加载，但是克隆的虚拟机系统里面的文件是没有替换的，故而需要手动取更改。

```shell
vi /etc/sysconfig/network-scripts/ifcfg-xxx
#更改mac地址为实际的虚拟机mac地址，更改后重启网络服务
service nerwork restart

```

如果还是没有ping通，则需要更改网卡配置的mac地址

```shell
vi /etc/udev/rules.d/70-persistent-net.rules

```

改好后重启网络服务，保险起见，重启系统最好。



# xshell使用心得

![jekyll.png](/upload/2022/01/jekyll-264a9f509230481fb7c8e548cc847d61.png)

## 多窗口



## 同步操作







## tesseract




对tesseract的使用源于项目开发中要将PDF文件转换为双层PDF，然后提取文件内容，存入ES供文档检索使用。本文粗略地记录了Tesseract的搭建和使用。

PDF --> tiff --> 双层PDF

1. imagemagick 转PDF为TIFF
2. tesseract 转TIFF为双层PDF

3. 安装tesseract

```shell
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
```

2. 操作命令operation

```shell
# PDF转TIFF
magick convert test.pdf -density 300 -depth 8 -strip -background white -alpha off image.tiff
# TIFF转双层PDF
tesseract -l chi_sim test.tiff test pdf 

magick convert E:\test\data\test.png -density 300 -depth 8 -strip -background white -alpha off E:\test\data\test.png.tiff 

magick convert -geometry 3600x3600 -density 300 -quality 100 -depth 8 -strip -background white -alpha off test.pdf test.tiff

tesseract -l chi_sim com.cyvation.exp0.tif com.cyvation.exp0  batch.nochop makebox

tesseract cyvation.stsong.exp0.tif cyvation.stsong.exp0 nobatch box.train

```

3. Java dependency

4. pdfbox 读取双层PDF文本
5. tesseract-platform TIFF转双层PDF
6. im4java/jmagick PDF转TIFF

7. 训练

工具:

jTessBoxEditor 

imagemagick

步骤:

1. 转tif格式图片

   命名规范[lang].[fontname].exp[num].tif  

   ```shell
   magick convert -geometry 3600x3600 -density 300 -quality 100 lang.font.exp0.pdf lang.font.exp0.tif
   ```

2. 生成box文件

```shell
tesseract -l chi_sim lang.font.exp0.tif lang.font.exp0 batch.nochop makebox

# 用jTEssBoxEditor编辑box文件
```

3. 生成tr训练数据

```shell
tesseract lang.font.exp0.tif lang.font.exp0 nobatch box.train
```

4. 生成unicharset

```shell
unicharset_extractor lang.font.exp0.box
```

5. 生成字库文件lang.font_properties

```shell
echo "font 0 0 0 0 0" > lang.font_properties
```

6. mftraining

```shell
mftraining -F lang.font_properties -U unicharset -O lang.unicharset lang.font.exp0.tr
```

7. cntraining

```shell
cntraining lang.font.exp0.tr

```

8. 更改文件名

```shell
mv inttemp lang.inttemp
mv pffmtable lang.pffmtable
mv normproto lang.normproto
mv shapetable lang.shapetable

```

9. 生成tessdata文件

```shell
combine_tessdata lang.

```

10. 拷贝训练好的数据到tessdata文件目录

11. 同时使用多个字体库提取文本

```shell
# 使用+链接语言,没有空格
tesseract -l chi_sim+por aaaa.tif out 

```

12. Tess4j

设置环境变量

TESSDATA_PREFIX	C:\jTessBoxEditorFX\tesseract-ocr\tessdata



## sublime



# Sublime3 的使用记录



Ctrl+Shift+D 复制一行

