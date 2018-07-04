# Spring Boot 学习笔记

本项目是学习Spring Boot时的笔记。学习自：**[原作者传送门](https://zdran.com/20180703.html)**

+ [Spring Boot 学习笔记 源码地址](https://github.com/zdRan/learning)
+ [Spring Boot 学习笔记(一) hello world](https://zdran.com/20180628.html)
+ [Spring Boot 学习笔记(二) 整合 log4j2](https://zdran.com/20180629.html)
+ [Spring Boot 学习笔记(三) 整合 MyBatis + Druid](https://zdran.com/20180703.html)

使用时记得修改application.yml中的数据库用户名+密码，还有log4j2.xml当中的日志存储地址，类似于下面：

`application.xml`

```properties
server:
  port: 9090
  servlet:
    context-path: /learning

mybatis:
  mapperLocations: classpath:mybatis/mapping/*.xml
  typeAliasesPackage: com.zdran.springboot.dao

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/LEARNING
    username: root
    password: admin
    driver-class-name: com.mysql.jdbc.Driver
    initialSize: 5
    minIdle: 5
    maxActive: 10
    maxWait: 10000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    testOnBorrow: false
    testOnReturn: false
    testWhileIdle: true
    keepAlive: true
    removeAbandoned: true
    removeAbandonedTimeout: 80
    logAbandoned: true
    poolPreparedStatements: true
    maxPoolPreparedStatementPerConnectionSize: 20
    filters: stat,slf4j,wall

```

`log4j2.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    status : 这个用于设置log4j2自身内部的信息输出,可以不设置,当设置成trace时,会看到log4j2内部各种详细输出
    monitorInterval : Log4j能够自动检测修改配置文件和重新配置本身, 设置间隔秒数。此处表示每隔600秒重读一次配置文件
-->
<Configuration status="OFF" monitorInterval="600">

    <!--日志级别：TRACE < DEBUG < INFO < WARN < ERROR < FATAL-->
    <!--如果设置为WARN，则低于WARN的信息都不会输出-->
    <Properties>
        <!-- 配置日志文件输出目录,此处为项目根目录下的logs文件夹 -->
        <Property name="LOG_HOME">D:\my_project\idea_pro_inuse\springboot5\log</Property>
        <property name="FILE_NAME">learningSpringBoot</property>
    </Properties>

    <Appenders>
        <!--这个输出控制台的配置-->
        <Console name="Console" target="SYSTEM_OUT">
            <!--控制台只输出level及其以上级别的信息（onMatch），其他的直接拒绝（onMismatch）-->
            <ThresholdFilter level="INFO" onMatch="ACCEPT" onMismatch="DENY"/>
            <!--日志输出的格式-->
            <!--%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n-->
            <PatternLayout pattern="|%d{yyyy-MM-dd HH:mm:ss.SSS}|%5p|%5t|%4c:%L|%m%n"/>
        </Console>

        <RollingRandomAccessFile name="LEARNING" fileName="${LOG_HOME}/${FILE_NAME}.log"
                                 filePattern="${LOG_HOME}/backup/${FILE_NAME}_%d{yyyy-MM-dd}_%i.log">
            <!--%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n-->
            <PatternLayout pattern="|%d{yyyy-MM-dd HH:mm:ss.SSS}|%5p|%5t|%4c:%L|%m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy interval="1"/>
                <SizeBasedTriggeringPolicy size="20MB"/>
            </Policies>
            <DefaultRolloverStrategy max="10"/>
        </RollingRandomAccessFile>
    </Appenders>

    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <Loggers>
        <Logger name="com.zdran.springboot" level="INFO" additivity="true">
            <AppenderRef ref="LEARNING"/>
        </Logger>
        <Logger name="org.springframework" level="INFO" additivity="true">
            <AppenderRef ref="LEARNING"/>
        </Logger>
        <Logger name="org.apache" level="INFO" additivity="true">
            <AppenderRef ref="LEARNING"/>
        </Logger>
        <Root level="INFO">
            <Appender-Ref ref="Console"/>
        </Root>
    </Loggers>

</Configuration>
```

