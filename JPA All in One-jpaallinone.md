---
title: JPA All in One
date: 2021-11-05 23:32:05.424
updated: 2021-11-05 23:32:05.424
url: https://halo.imjcker.com/archives/jpaallinone
categories: 
tags: jpa
---


1. 调研对比Hibernate、OpenJPA、Eclipselink、Toplink Essentials等JPA实现的性能。

## QueryDSL

## spring boot jpa动态数据源

**AbstractRoutingDataSource**

**方案：**  在一个确切的地方存储数据源的配置信息（主数据源）。启动spring时，会初始化这个配置数据源，然后将其他动态数据源信息取出来初始化好datasource 注册到spring 容器。

**原理：** 主要是实现AbstractRoutingDataSource的抽象类，然后将该类注册到spring容器。

**要点：** 

1. 配置`AbstractRoutingDataSource` 类的默认数据源`Object defaultTargetDataSource` 和其他数据源`Map<Object, Object> targetDataSources`。targetDataSources就是我们动态配置的数据源，key-value 接口，后面根据key 查找 datasource。
2. 实现determineCurrentLookupKey()方法，该方法决定了当前操作选择哪个数据源。
3. 注册到spring 容器，在配置类中注入`AbstractRoutingDataSource`的实现类。

**如何确定数据源的选择：** 业务层面通过API request中附带的参数（header、session、cookie、url_param等信息）来判断此次请求对应的数据源是哪个？例如 url?appid=1 ，那么就判断此次请求是appid=1 的应用库。第二步，确定后，将appid=1 对应的数据源key 存入本地线程ThreadLocal中。后面在determineCurrentLookupKey方法中 从本地线程ThreadLocal中取出对应的key。spring 会根据该key 选择对应的datasource 作为接下来操作的数据源。

**demo**说明

实现AbstractRoutingDataSource

```java
/**
 * @author thh 2019-02-22
 * @version 1.0.0
 * description: 动态路由数据源实现类
 **/
public class DynamicDataSource extends AbstractRoutingDataSource {
    @Override
    protected Object determineCurrentLookupKey() {
        return DataSourceContextHolder.getDataSource();
    }

    private Map<Object, Object> targetDataSourcesBackup = new ConcurrentHashMap<>();

    public void setTargetDataSourcesBackup(Map targetDataSources) {
        Assert.notNull(targetDataSources, "TargetDataSources can not be null");
        this.targetDataSourcesBackup.putAll(targetDataSources);
    }

    public void addTargetDataSource(String key, DataSource ds) {
        this.targetDataSourcesBackup.put(key, ds);
        this.setTargetDataSources(this.targetDataSourcesBackup);
        this.afterPropertiesSet();
    }
}
```