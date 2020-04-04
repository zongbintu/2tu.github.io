title: annotationProcessor 引用后编译时不生效问题 
tags: [annotationProcessor,AutoService,Gradle]
date: 2020-04-04 22:16:28
categories: Android
---
####  Gradle升级后Processor无法在编译时生效  

升级[fit](https://github.com/2tu/fit)后忽然发现在annotaionProcessor后编译并没有生成。原因在哪里呢？将fit库的gradle版本从4.6升级到了5.6.4，原来从gradle4.7开始不再帮助生成，需要我们自己在库中解决。    
#### 两种方案
<!-- more -->

#####	1、processor中添加anntationProcessor
在comiple库的build.gradle中加入后，其它不需要动即可（我的库使用的AutoServerice）

```
annotationProcessor deps.autoservice
```

#####	2、自己写一个gradle build plugin
需要用户使用时引入gradle plugin方能生效  

```
apply plugin: 'custom.plugin'
```

#### 参考  
 [Incremental annotation processing](https://docs.gradle.org/nightly/userguide/java_plugin.html#sec:incremental_annotation_processing)
 [annotation processor 为啥没有被调用?](https://www.cnblogs.com/mengshu-lbq/p/11713397.html)
 [Annotation processor @autoservice](https://stackoverflow.com/questions/44530648/annotation-processor-autoservice)
[Butter Knife](https://github.com/JakeWharton/butterknife#butter-knife)


###### 转载请标明出处： 
###### http://blog.520wa.com/
###### 本文出自Tu's blog