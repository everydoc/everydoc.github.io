---
title: Java基础必修.md
date: 2021-11-05 23:31:20.981
updated: 2022-01-19 01:04:11.063
url: https://halo.imjcker.com/archives/java基础必修md
categories: 
tags: 
---


```java
public class IntrospectorDemo {
    private String name;
    public static void main(String[] args) throws Exception{
        IntrospectorDemo demo = new IntrospectorDemo();
        demo.setName( "Winter Lau" );
        // 如果不想把父类的属性也列出来的话， getBeanInfo 的第二个参数填写父类的信息
        BeanInfo bi = Introspector.getBeanInfo(demo.getClass(), Object. class );
        PropertyDescriptor[] props = bi.getPropertyDescriptors();
        for ( int i=0;i<props.length;i++){
            System.out.println(props[i].getName()+ "=" + props[i].getReadMethod().invoke(demo, null ));
        }
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this .name = name;
    }
}

```

## JDK环境变量问题

在同一台电脑安装多个版本的jdk，然后又配置了环境变量，而后又因为其它原因需要更改环境变量，发现怎么改配置都不起效，可能需要到Windows/system32下删除一些java.exe, java.exe文件，但是也不一定能解决这个问题，如果依然不起效，接下来的操作是：到环境变量的path中，将jdk的配置移动到最前面，这时应该就会生效了。


## Java集合框架

**前言：** 面试了几家公司，发现自己Java涉及到的知识面广度稍有，但是深度不够。故此重游Java基础中的重要模块。  

Collection

> List
>
> > ArrayList
> >
> > LinkedList
> >
> > Vector
> >
> > > Stack
>
> Set
>
> > HashSet
> >
> > Queue

Map

> HashTable
>
> HashMap
>
> TreeMap
>
> WeakHashMap



上面大体提到了一些常用的集合。  

**ClassLoader**

什么是ClassLoader

ClassLoader是一个抽象类，我们用它的实例对象来装载类 ，它负责将 Java 字节码装载到 JVM 中 ， 并使其成为 JVM 一部分。 JVM 的类动态装载技术能够在运行时刻动态地加载或者替换系统的某些功能模块，而不影响系统其他功能模块的正常运行。一般是通过类名读入一个class文件来装载这个类，（其它加载形式暂时没有研究过）。

ClassLoader装载过程

类装载就是寻找一个类或是一个接口的字节码文件并通过解析该字节码来构造代表这个类或是这个接口的 class 对象的过程 。在 Java 中，类装载器把一个类装入 Java 虚拟机中，要经过三个步骤来完成：装载、链接和初始化，其中链接又可以分成校验、准备和解析三步，除了解析外，其它步骤是严格按照顺序完成的，各个步骤的主要工作如下：

1. 装载：查找和导入类或接口的字节码；
2. 链接：执行下面的校验、准备和解析步骤，其中解析步骤是可以选择的
   1. 校验：检查导入类或接口的二进制数据的正确性；
   2. 准备：给类的静态变量分配并初始化存储空间；
   3. 解析：将符号引用转成直接引用;
3. 初始化：激活类的静态变量的初始化 Java 代码和静态 Java 代码块。

 

装载的实现

JVM 中类的装载是由 ClassLoader 和它的子类来实现的。 Java ClassLoader 是一个重要的 Java 运行时系统组件，它负责在运行时查找和装入 Java 字节码。

在 Java 中， ClassLoader 是一个抽象类，它在包 java.lang 中。可以这样说，只要了解了 ClassLoader 中的一些重要的方法，再结合上面所介绍的 JVM 中类装载的具体的过程，对动态装载类这项技术就有了一个比较大概的掌握，这些重要的方法包括以下几个：

1. loadCass 方法： loadClass(String name ,boolean resolve) 其中 name 参数指定了 JVM 需要的类的名称 , 该名称以类的全限定名表示，如 Java.lang.Object ； resolve 参数告诉方法是否需要解析类，在初始化类之前，应考虑类解析，并不是所有的类都需要解析，如果 JVM 只需要知道该类是否存在或找出该类的超类，那么就不需要解析。这个方法是 ClassLoader 的入口点。
2. defineClass 方法   这个方法接受类文件的字节数组并把它转换成 Class 对象。字节数组可以是从本地文件系统或网络装入的数据。它把字节码分析成运行时数据结构、校验有效性等等。
3. findSystemClass 方法   findSystemClass 方法从本地文件系统装入 Java 字节码。它在本地文件系统中寻找类文件，如果存在，就使用 defineClass 将字节数组转换成 Class 对象。当运行 Java 应用程序时 , 这是 JVM 正常装入类的缺省机制。
4. resolveClass 方法 resolveClass(Class c) 方法解析装入的类，如果该类已经被解析过那么将不做处理。当调用 loadClass 方法时 , 通过它的 resolve 参数决定是否要进行解析。
5. findLoadedClass 方法   当调用 loadClass 方法装入类时 , 调用 findLoadedClass 方法来查看 ClassLoader 是否已装入这个类 , 如果已装入 , 那么返回 Class 对象 , 否则返回 NULL 。如果强行装载已存在的类 , 将会抛出链接错误。 

## Effective Java

1. 对于非公有的方法，通常应该使用assertion来检查他们的参数，而不使用正常的检查语句
2. 只要能够不创建对象，就不要创建多余的对象。只要需要创建对象，就不要吝啬地创建它。它们并不矛盾，前者的目的是为了尽可能减少资源的占用，提高运行效率，后者是为了安全性（保护性拷贝）。

## Java agent 探针技术

### 什么是Java agent

Java agent 是一种可以动态修改Java字节码的技术。Java类编译之后形成字节码被JVM执行，JVM在执行这些字节码之前获取这些字节码信息，并且对这些字节码进行修改，来完成一些额外的功能，这种就是java agent技术。

### Java agent能做什么

通过以上两种就可以实现在一些框架或是技术的采集点进行字节码修改，可以对应用进行监控，或是对执行指定方法或是接口时额外添加操作（打印日志、打印方法执行时间、采集方法的入参和结果等）

### Java agent原理

在了解了Java类加载机制的起初上，接下来我们分别介绍JVMTI和JVMTIAgent。

#### JVMTI

JVMTI是JVM Tool Interface的缩写，是JVM暴露出来给用户扩展使用的接口集合，JVMTI是基于事件驱动的，JVM每执行一定的逻辑就会调用一些事件的回调接口，这些接口可以给用户自行扩展来实现自己的逻辑.

#### JVMTIAgent

JVMTIAgent是一个动态库，利用JVMTI暴露出来的接口实现用户自行的逻辑（eclipse、idea等工具等代码调试就是通过这个实现的）

JVMTIAgent主要有三个方法，

**Agent_OnLoad**方法，如果agent在启动时加载，就执行这个方法。

**Agent_OnAttach**方法，如果agent不是在启动的时候加载的，是我们先attach到目标线程上，然后对对应的目标进程发送load命令来加载agent，在加载过程中调用Agent_OnAttach函数。

**Agent_OnUnload**方法，在agent做卸载掉时候调用。

### 开源框架



### 案例





## 函数式接口

FunctionalInterface





## Java8

JAVA8 stream接口 distinct，sorted，peek，limit，skip

https://blog.csdn.net/qq_28410283/article/details/80643711

JAVA8 Consumer接口

https://blog.csdn.net/qq_28410283/article/details/80618456

java8 Optional静态类简介，以及用法

https://blog.csdn.net/qq_28410283/article/details/80952768






## json处理

1. Java 转 JSONObject

   (JSONObject)JSONObject.toJSON(Java对象实例)

2. Java 转 Json字符串

   JSONObject.toJSONString(Java对象实例)

3. JSONObject 转 Json字符串

   JSONObject.toJSONString();

4. JSONObject 转 Java

   JSONObject.toJavaObject(JSON对象实例, Java对象.class);

5. Json字符串转JSONObject

   JSONObject.parseObject(JSON字符串)

6. Json字符串转Java

   JSONObject.parseObject(JSON字符串, Java对象.class);



# 并发编程的三大特性
![java.png](https://halo.imjcker.com/upload/2022/01/java-4b644c9631884d7bbcf7b23fdb17f4e7.png)
## 原子性
原子性指的是一个操作是不可中断的，即使在多线程环境下，一个操作一旦开始就不会被其他线程影响！
举例说明：比如一个变量的++操作，代码中只有一行 i++，但在虚拟机底层并不是一步完成的，执行引擎最少需要经过赋值、自增1、写入工作内存等操作才完成，在这期间可能cpu被其他线程抢去，不能保证操作的原子性！一行代码已经如此，更何况真实开发的几十行代码更有原子性问题！

## 可见性
可见性指的是当一个线程修改了某个共享变量的值，其他线程能够马上感知到这个修改的值。
举例说明：对于单线程来说，串行不存在可见性问题。对于多线程来说，由于不同线程对共享变量的操作都是在自己的工作内存中执行，这就可能存在 ：线程A在自己的工作内存中修改了共享变量x的值，还没来得及写回主内存中，线程B把主内存中原来的值又加载到自己的工作内存中进行操作，线程A对共享变量的操作对线程B来说不可见，这种工作内存与主内存同步的延迟现象就造成了可见性问题！另外指令重排以及编译器优化也可能导致可见性问题。

## 有序性
有序性即程序执行的顺序是否按照代码的先后顺序执行。对于单线程来说，可以认为代码是按照顺序执行的，所有操作都视为有序行为。但是编译器和内存存在指令重排的情况，指令重排虽然仍能保证单线程的有序性（语义一致性），但是却无法保证多线程情况下的有序性，从而导致多线程下程序出错。

## 指令重排
在保证程序最终结果一致、数据依赖性的前提下，JVM对机器指令进行重新排序。使代码指令更符合cpu执行特性，最大限度的发挥机器性能，提高程序运行效率。

