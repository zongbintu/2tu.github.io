title: 内存没满但是频繁fullgc，是不是ReservedCodeCacheSize的锅
categories: Java
tags: [fullgc]
date: 2023-09-03 10:38:35
---


##### 业务中的现象表现

运行一段时间后，频繁fullgc，2s10几次fullgc，无法对外提供服务

#### 环境

##### gc日志

内存还有非常多的情况下就开始了fullgc

##### dump分析

只看到class loader的一直重新加载？内存指向java.security.Permission，没有其他大对象？没有搞懂重新改好了也是这样，需要再搞懂

##### jvm参数

``` shell
-Xms128m -Xmx256m -XX:ReservedCodeCacheSize=50m -XX:MetaspaceSize=96m -XX:MaxMetaspaceSize=96m -Xss256k -XX:MaxDirectMemorySize=16m
-XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSClassUnloadingEnabled
-XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCDateStamps -XX:GCLogFileSize=10M -XX:NumberOfGCLogFiles=3 -Xloggc:/var/logs/gc1.log
-XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:NativeMemoryTracking=summary
-XX:+HeapDumpOnOutOfMemoryError  -XX:HeapDumpPath=/var/logs/dump1-$(date +%Y%m%d%H%M%S).hprof"

```

##### 修改

怀疑是-XX:ReservedCodeCacheSize=50m导致Meta不生效，导致重新加载fullgc?

修改为jvm参数，后好使了

``` shell
-Xms128m -Xmx256m -XX:PermSize=128m -XX:-UseGCOverheadLimit -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=80
-XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCDateStamps -XX:GCLogFileSize=10M -XX:NumberOfGCLogFiles=3 -Xloggc:/var/logs/gc1.log
-XX:CMSInitiatingOccupancyFraction=75 -XX:+UseCMSInitiatingOccupancyOnly -XX:NativeMemoryTracking=summary
-XX:+HeapDumpOnOutOfMemoryError  -XX:HeapDumpPath=/var/logs/dump1-$(date +%Y%m%d%H%M%S).hprof"
```



需要再研究，怎么来指向这个问题。

另外火焰图必不可少
