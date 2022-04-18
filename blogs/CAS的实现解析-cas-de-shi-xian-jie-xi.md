---
title: CAS的实现解析
date: 2022-01-11 00:25:20.432
updated: 2022-01-13 02:06:35.285
url: https://halo.imjcker.com/archives/cas-de-shi-xian-jie-xi
categories: Java SE
tags: cas | Java | javase
---

# CAS的实现解析
![Java](/upload/2022/01/java-4b644c9631884d7bbcf7b23fdb17f4e7.png)

CAS顾名思义：Compare And Swap
意思是比较并替换，
它首先是依赖Java提供的rt.jar包中的Unsafe类来工作的；
而Unsafe又是一个native类，
用于调用底层系统的操作；
底层系统又是通过CPU原语句来实现原子操作的；
CPU原语具有连续性和不可分割打断的特性；
从上自下，一步步实现了CAS。

## 缺陷
可能导致循环时间过长；
只能保证一个共享变量的原子操作；
造成ABA问题。

## ABA问题
CAS的运用会造成ABA问题，
所谓的ABA问题可以简单地描述为：
一个线程将初始值为A的一个变量改为B，而后又改为A，
在这个时间段后另一个线程也对A进行CAS操作，
这时候会发现这个变量还是A未变，
从而CAS成功，
但实际上另一个线程是更改过这个变量的。

在某些场景中ABA问题无所谓，
但是在另一些场景中是无法忽略中间被更改过的变量的。

## 挽救ABA问题
添加版本号，
一旦有线程使用过该变量就修改版本号，
从而从逻辑上实现独占效果，避免ABA问题。


## 参考
https://elsef.com/2020/03/08/如何理解ABA问题/
