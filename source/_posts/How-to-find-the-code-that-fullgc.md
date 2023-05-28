title: 如何找到导致fullgc的问题代码
tags: [fullgc]
date: 2023-05-28 08:50:28
categories: Java
---
服务频繁fullgc无法对外提供服务。需要查看分析出是大对象？内存分配不合理还是造成了内存泄漏的问题。
查找步骤

- 查看，确认gc情况
- 查看jvm运行参数

- dump分析
- 找出问题代码

- 查看存活对象情况
- 查看该进程下占用CPU最高的线程

#### 查看，确认gc情况
```shell
jstat -gc [pid] 2000
```
每2秒打印一次pid的gc情况
![jstat gc](jstat_gc.jpg)

<!-- more -->

#### 查看jvm运行参数
```shell
jinfo -flags [pid]
```

#### dump分析
```shell
jmap -dump:live,format=b,file=[filename].hporf [pid]
```

#### 使用MAT分析

##### 内存情况
![Overview](thread_overview.png)
##### 查看对象Histogram
![Histogram](thread_histogram.png)
##### 大对象Dominator Tree
![Dominator Tree](thread_dominator_tree.png)