---
title: Log All in One
date: 2021-11-05 23:32:13.173
updated: 2021-11-05 23:32:13.173
url: https://halo.imjcker.com/archives/logallinone
categories: 
tags: log 日志
---


各种日志操作记录。

1. 目的

通过调研对比，选取适合统一业务2.0业务场景的商用日志服务。

2. 阿里云日志服务

**采集方式**

LogHub 支持客户端、网页、协议、SDK/API等多种日志无损采集方式，所有采集方式均基于Restful API实现，除此之外您也可以通过API/SDK实现新的采集方式。

**数据来源**

当前，日志服务支持采集以下来源的日志数据：

| 类别             | 来源                                                         | 接入方式                                                     | 更多                                                         |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 应用             | 程序输出                                                     | [Logtail](https://help.aliyun.com/document_detail/28979.html) | [案例](https://help.aliyun.com/document_detail/59355.html)   |
| 访问日志         | [Logtail](https://help.aliyun.com/document_detail/28979.html) | [采集并分析Nginx访问日志](https://help.aliyun.com/document_detail/56728.html) |                                                              |
| 链路跟踪         | Jaeger Collector，[Logtail](https://help.aliyun.com/document_detail/28979.html) | -                                                            |                                                              |
| 语言             | Java                                                         | [SDK](https://help.aliyun.com/document_detail/29063.html)，[Java Producer Library](https://help.aliyun.com/document_detail/43758.html) | -                                                            |
| Log4J Appender   | [1.x](https://github.com/aliyun/aliyun-log-log4j-appender)，[2.x](https://github.com/aliyun/aliyun-log-log4j2-appender) | -                                                            |                                                              |
| LogBack Appender | [LogBack](https://github.com/aliyun/aliyun-log-logback-appender) | -                                                            |                                                              |
| C                | [Native](https://help.aliyun.com/document_detail/29063.html) | -                                                            |                                                              |
| Python           | [Python](https://help.aliyun.com/document_detail/29063.html) | -                                                            |                                                              |
| Python Logging   | [Python Logging Handler](https://aliyun-log-python-sdk.readthedocs.io/tutorials/tutorial_logging_handler.html) | -                                                            |                                                              |
| PHP              | [PHP](https://help.aliyun.com/document_detail/29063.html)    | -                                                            |                                                              |
| C#               | [C#](https://help.aliyun.com/document_detail/29063.html)     | -                                                            |                                                              |
| C++              | [C++ SDK](https://help.aliyun.com/document_detail/108042.html) | -                                                            |                                                              |
| Go               | [Go](https://help.aliyun.com/document_detail/29063.html)     | -                                                            |                                                              |
| NodeJS           | [NodeJs](https://github.com/aliyun-UED/aliyun-sdk-js)        | -                                                            |                                                              |
| JS               | [JS/Web Tracking](https://help.aliyun.com/document_detail/31752.html) | -                                                            |                                                              |
| OS               | Linux                                                        | [Logtail](https://help.aliyun.com/document_detail/28979.html) | -                                                            |
| Windows          | [Logtail](https://help.aliyun.com/document_detail/28979.html) | -                                                            |                                                              |
| Mac/Unix         | [Native C](https://help.aliyun.com/document_detail/29063.html) | -                                                            |                                                              |
| Docker文件       | [Logtail 文件采集](https://help.aliyun.com/document_detail/66655.html) | -                                                            |                                                              |
| Docker输出       | [Logtail 容器输出](https://help.aliyun.com/document_detail/66658.html) | -                                                            |                                                              |
| 数据库           | MySQL Binlog                                                 | [MySQL Binlog方式](https://help.aliyun.com/document_detail/64953.html) | -                                                            |
| JDBC Select      | [JDBC查询结果](https://help.aliyun.com/document_detail/64955.html) | -                                                            |                                                              |
| 移动端           | iOS/Android                                                  | [iOS SDK](https://help.aliyun.com/document_detail/43145.html)，[Android SDK](https://help.aliyun.com/document_detail/43200.html) | -                                                            |
| 网页             | [JS/Web Tracking](https://help.aliyun.com/document_detail/31752.html) | -                                                            |                                                              |
| 智能IoT          | [C Producer Library](https://github.com/aliyun/aliyun-log-c-sdk) | [案例](https://help.aliyun.com/document_detail/64558.html)   |                                                              |
| 其他             | HTTP 轮询                                                    | [Logtail HTTP](https://help.aliyun.com/document_detail/64954.html) | [Nginx监控](https://help.aliyun.com/document_detail/65673.html) |
| Syslog           | [Logtail插件-syslog输入源](https://help.aliyun.com/document_detail/89509.html) | -                                                            |                                                              |
| 云产品           | ECS、OSS等多个云产品。详细清单请参考[云产品日志](https://help.aliyun.com/document_detail/28981.html?spm=a2c4g.11186623.4.6.44ed735dBjnHsR#table-of3-xh4-vgb)。 | 云产品控制台开通。                                           | [云产品日志](https://help.aliyun.com/document_detail/107840.html) |
| 第三方           | Logstash                                                     | [Logstash](https://help.aliyun.com/document_detail/28984.html) | -                                                            |

**功能优势**

- 基于日志文件、无侵入式的收集日志。用户无需修改应用程序代码，且日志收集不会影响用户应用程序的运行逻辑。
- 除支持文本日志采集外，还支持binlog、http、容器stdout等采集方式。
- 对于容器支持友好，支持标准容器、swarm集群、Kubernetes集群等容器集群的数据采集。
- 能够稳定地处理日志收集过程中各种异常。当遇到网络异常、服务端异常等问题时会采用主动重试、本地缓存数据等措施保障数据安全。
- 基于服务端的集中管理能力。用户在安装Logtail后（参见 [安装Logtail（Windows系统）](https://help.aliyun.com/document_detail/49006.html?spm=a2c4g.11186623.2.15.16be216cbRRCWv) 和 [安装Logtail（Linux系统）](https://help.aliyun.com/document_detail/28982.html?spm=a2c4g.11186623.2.16.16be216cbRRCWv)），只需要在服务端集中配置需要收集的机器、收集方式等信息即可，无需逐个登录服务器进行配置。
- 完善的自我保护机制。为保证运行在客户机器上的收集Agent不会明显影响用户自身服务的性能，Logtail客户端在CPU、内存及网络使用方面都做了严格的限制和保护机制。



# Log4j2

**依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
<dependency>	<!-- 加上这个才能辨认到log4j2.yml文件 -->
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-yaml</artifactId>
</dependency>
```

**yaml**配置

```yaml
Configuration:
  status: debug

  Properties: # 定义全局变量
    Property: # 缺省配置（用于开发环境）。其他环境需要在VM参数中指定，如下：
    #测试：-Dlog.level.console=warn -Dlog.level.xjj=trace
    #生产：-Dlog.level.console=warn -Dlog.level.xjj=info
    - name: log.level.console
      value: trace
    - name: log.level.imjcker
      value: trace
    - name: log.path
      value: E:/Logs/demo-es
    - name: project.name
      value: my-spring-boot

  Appenders:
    Console:  #输出到控制台
      name: CONSOLE
      target: SYSTEM_OUT
      ThresholdFilter:
        level: ${sys:log.level.console} # “sys:”表示：如果VM参数中没指定这个变量值，则使用本文件中定义的缺省全局变量值
        onMatch: ACCEPT
        onMismatch: DENY
      PatternLayout:
        pattern: "%d{yyyy-MM-dd HH:mm:ss,SSS}:%4p %t (%F:%L) - %m%n"
#    RollingFile: # 输出到文件，超过128MB归档
#    - name: ROLLING_FILE
#      ignoreExceptions: false
#      fileName: ${log.path}/${project.name}.log
#      filePattern: "${log.path}/$${date:yyyy-MM}/${project.name}-%d{yyyy-MM-dd}-%i.log.gz"
#      PatternLayout:
#        pattern: "%d{yyyy-MM-dd HH:mm:ss,SSS}:%4p %t (%F:%L) - %m%n"
#      Policies:
#        SizeBasedTriggeringPolicy:
#          size: "128 MB"
#      DefaultRolloverStrategy:
#        max: 1000

  Loggers:
    Root:
      level: info
      AppenderRef:
      - ref: CONSOLE
      - ref: ROLLING_FILE
    Logger: # 为com.xjj包配置特殊的Log级别，方便调试
    - name: com.imjcker.demo
      additivity: false
      level: ${sys:log.level.imjcker}
      AppenderRef:
      - ref: CONSOLE
#      - ref: ROLLING_FILE
```





# pinpoint

1. 下载

<https://github.com/naver/pinpoint/releases>

2. 部署

将web和collector的war包拷贝到tomcat的webapps下，启动tomcat。

**搭建gitlab**

```shell
docker pull gitlab/gitlab-ce

```

docker-compose

```yaml
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'imjcker.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://gitlab.example.com'
      # Add any other gitlab.rb configuration here, each on its own line
  ports:
    - '80:80'
    - '443:443'
    - '22:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
```



# logback配置

<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径 -->
    <property name="LOG_HOME" value="${user.home}/logs" />
    <!-- 控制台输出 -->
    <appender name="WebLog" class="com.imjcker.weblogger.logging.WebLogAppender">
<!--        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            &lt;!&ndash; 日志收集最低日志级别 &ndash;&gt;
            <level>INFO</level>
        </filter>-->
<!--        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>-->
    </appender>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>
    
    <!-- 自定义包下设置为INFO,则可以看见输出的日志不包含debug输出了 -->
    <logger name="com.imjcker.weblogger" level="INFO" />
    <logger name="org.springframework" level="INFO" />
    
    <!-- 日志输出级别 -->
    <root level="INFO">
        <appender-ref ref="WebLog" />
        <appender-ref ref="STDOUT" />
    </root>

</configuration>