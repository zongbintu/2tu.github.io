title: 如何启动Phoenix Query Server
categories: Data
tags: [Phoenix Query Server 6.0.0]
date: 2023-08-03 21:57:55
---





### 问题

启动Phenix Query Server 6.0.0报错

```shell
2023-08-03 15:20:01.672088 launching /var/jdk/bin/java -cp /etc/hbase/conf:/etc/hadoop/conf::/var/phoenix-queryserver-6.0.0/bin/../phoenix-queryserver-6.0.0.jar: -Dproc_phoenixserver -Dlog4j.configuration=file:/var/phoenix-queryserver-6.0.0/bin/log4j.properties -Dpsql.root.logger=INFO,DRFA -Dpsql.log.dir=/tmp/phoenix -Dpsql.log.file=phoenix-root-queryserver.log org.apache.phoenix.queryserver.server.QueryServer

错误: 找不到或无法加载主类 org.apache.phoenix.queryserver.server.QueryServer
```

HBase 2.4.15

Phoenix 5.1.3

### 前因

升级HBase 2.4.15后，之前的Phoenix 4.13.1就不能用了。从Phoenix官网看到Phoenix和HBase的版本对应。

> Current release 4.16.1 can run on Apache HBase 1.3, 1.4, 1.5 and 1.6.
> Current release 5.1.3 can run on Apache HBase 2.1, 2.2, 2.3, 2.4 and 2.5.

然后Phoenix 5.1.3中没有带query server，需要单独下载query server并启动，如图

![Query Server Installation](query-server-installation.png)

<!-- more -->

### 原因分析

下载启动[Phonex Query 6.0.0](https://phoenix.apache.org/download.html)报了上面的错误

#### 找不到或无法加载主类

报这种错，原因有可能是

- class path路径不对

- 子路径不匹配

- class path缺少依赖

  class-path需要你整个应用所有的依赖的class，也就是为了主类加载正确，JVM需要找到：

  - 主类本身；
  - 所有父亲类以及接口；
  - 所有声明变量的类以及调用的方法等

查看上面的错误日志phoenix-queryserver-6.0.0.jar中是有org.apache.phoenix.queryserver.server.QueryServer的，可能的问题是class path缺少依赖

#### 修改日志级别，查看详细错误信息

需要找到具体缺少了什么依赖，能否拿到详细报错信息辅助我们找到错误？

启动命令中有log4j.properties -Dpsql.root.logger=INFO,

我选择修改log3j.properties中的日志类型为DEBUG，再重新启动，查看日志，得到如下错误信息

```shell
2023-08-03 15:28:01.024619 launching /var/jdk/bin/java -cp /etc/hbase/conf:/etc/hadoop/conf:/var/phoenix-queryserver-6.0.0/bin/../phoenix-client-hbase-2.4-5.1.3.jar:/var/phoenix-queryserver-6.0.0/bin/../phoenix-queryserver-6.0.0.jar: -Dproc_phoenixserver -Dlog4j.configuration=file:/var/phoenix-queryserver-6.0.0/bin/log4j.properties -Dpsql.root.logger=INFO,DRFA -Dpsql.log.dir=/tmp/phoenix -Dpsql.log.file=phoenix-root-queryserver.log  org.apache.phoenix.queryserver.server.QueryServer
Exception in thread "Thread-20" java.lang.RuntimeException: java.io.FileNotFoundException: /var/phoenix-queryserver-6.0.0/phoenix-client-hbase-2.4-5.1.3.jar (没有那个文件或目录)
        at org.apache.hadoop.conf.Configuration.loadResource(Configuration.java:3021)
        at org.apache.hadoop.conf.Configuration.loadResources(Configuration.java:2968)
        at org.apache.hadoop.conf.Configuration.getProps(Configuration.java:2848)
        at org.apache.hadoop.conf.Configuration.get(Configuration.java:1200)
        at org.apache.hadoop.conf.Configuration.getTrimmed(Configuration.java:1254)
        at org.apache.hadoop.conf.Configuration.getBoolean(Configuration.java:1660)
        at org.apache.hadoop.hbase.HBaseConfiguration.checkDefaultsVersion(HBaseConfiguration.java:69)
        at org.apache.hadoop.hbase.HBaseConfiguration.addHbaseResources(HBaseConfiguration.java:83)
        at org.apache.hadoop.hbase.HBaseConfiguration.create(HBaseConfiguration.java:97)
        at org.apache.phoenix.query.ConfigurationFactory$ConfigurationFactoryImpl$1.call(ConfigurationFactory.java:49)
        at org.apache.phoenix.query.ConfigurationFactory$ConfigurationFactoryImpl$1.call(ConfigurationFactory.java:46)
        at org.apache.phoenix.util.PhoenixContextExecutor.call(PhoenixContextExecutor.java:76)
        at org.apache.phoenix.util.PhoenixContextExecutor.callWithoutPropagation(PhoenixContextExecutor.java:90)
        at org.apache.phoenix.query.ConfigurationFactory$ConfigurationFactoryImpl.getConfiguration(ConfigurationFactory.java:46)
        at org.apache.phoenix.jdbc.PhoenixDriver$1.run(PhoenixDriver.java:77)
Caused by: java.io.FileNotFoundException: /var/phoenix-queryserver-6.0.0/phoenix-client-hbase-2.4-5.1.3.jar (没有那个文件或目录)
        at java.util.zip.ZipFile.open(Native Method)
        at java.util.zip.ZipFile.<init>(ZipFile.java:225)
        at java.util.zip.ZipFile.<init>(ZipFile.java:155)
        at java.util.jar.JarFile.<init>(JarFile.java:166)
        at java.util.jar.JarFile.<init>(JarFile.java:103)
        at sun.net.www.protocol.jar.URLJarFile.<init>(URLJarFile.java:93)
        at sun.net.www.protocol.jar.URLJarFile.getJarFile(URLJarFile.java:69)
        at sun.net.www.protocol.jar.JarFileFactory.get(JarFileFactory.java:99)
        at sun.net.www.protocol.jar.JarURLConnection.connect(JarURLConnection.java:122)
        at sun.net.www.protocol.jar.JarURLConnection.getInputStream(JarURLConnection.java:152)
        at org.apache.hadoop.conf.Configuration.parse(Configuration.java:2943)
        at org.apache.hadoop.conf.Configuration.getStreamReader(Configuration.java:3037)
        at org.apache.hadoop.conf.Configuration.loadResource(Configuration.java:2995)
        ... 14 more
2023-08-03 17:44:55.845183 launching /var/jdk/bin/java -cp /etc/hbase/conf:/etc/hadoop/conf::/var/phoenix-queryserver-6.0.0/bin/../phoenix-queryserver-6.0.0.jar: -Dproc_phoenixserver -Dlog4j.configuration=file:/var/phoenix-queryserver-6.0.0/bin/log4j.properties -Dpsql.root.logger=INFO,DRFA -Dpsql.log.dir=/tmp/phoenix -Dpsql.log.file=phoenix-root-queryserver.log  org.apache.phoenix.queryserver.server.QueryServer
错误: 找不到或无法加载主类 org.apache.phoenix.queryserver.server.QueryServer

```

从上面日志，看到了错误问题，缺少phoenix-client-hbase-2.4-5.1.3.jar

### 解决问题

#### 复制phoenix-client-hbase-2.4-5.1.3.jar

从Phoenix 5.1.3目录下复制phoenix-client-hbase-2.4-5.1.3.jar到phoenix-queryserver-6.0.0的目录下



#### 启动Phoenix Query Server

```shell
bin/queryserver.py start
```

启动成功

```shell
[root@ump225 phoenix-queryserver-6.0.0]# bin/queryserver.py start
starting Query Server, logging to /tmp/phoenix/phoenix-root-queryserver.log
[root@ump225 phoenix-queryserver-6.0.0]# tail -100f /tmp/phoenix/phoenix-root-queryserver.out
2023-08-03 23:13:57.910434 launching /var/jdk/bin/java -cp /etc/hbase/conf:/etc/hadoop/conf:/var/phoenix-queryserver-6.0.0/bin/../phoenix-client-hbase-2.4-5.1.3.jar:/var/phoenix-queryserver-6.0.0/bin/../phoenix-queryserver-6.0.0.jar: -Dproc_phoenixserver -Dlog4j.configuration=file:/var/phoenix-queryserver-6.0.0/bin/log4j.properties -Dpsql.root.logger=INFO,DRFA -Dpsql.log.dir=/tmp/phoenix -Dpsql.log.file=phoenix-root-queryserver.log  org.apache.phoenix.queryserver.server.QueryServer
```

### 总结

如何启动Phoenix Query Server 6.0.0

- 从Phoenix 5.1.3目录下复制phoenix-client-hbase-2.4-5.1.3.jar到phoenix-queryserver-6.0.0的目录下
- 在Phoenix Query Server 6.0.0目录下执行bin/queryserver.py start
- 查看日志/tmp/phoenix/phoenix-root-queryserver.out是否启动成功
