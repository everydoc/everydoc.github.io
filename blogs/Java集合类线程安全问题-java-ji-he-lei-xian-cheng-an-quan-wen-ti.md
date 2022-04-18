---
title: Java集合类线程安全问题
date: 2022-01-11 12:52:59.361
updated: 2022-01-13 02:07:06.914
url: https://halo.imjcker.com/archives/java-ji-he-lei-xian-cheng-an-quan-wen-ti
categories: Java SE
tags: Java | javase | 集合
---

# Java集合线程安全问题
![java](/upload/2022/01/java-4b644c9631884d7bbcf7b23fdb17f4e7.png)
### ArrayList 线程不安全
#### ArrayList底层

#### ArrayList不安全原因
ArrayList的add方法没有加锁，会导致java.util.ConcurrentModificationException

### 解决
#### Vector

#### Collections.synchronizedList

#### CopyOnWriteArrayList

