title: Spring中Bean的作用域与StepScope关系、疑问  
date: 2018-01-12 22:39:39  
tags: [Java,Spring,SpringBatch,Bean,scope,Qualifier,StepScope]
---
Spring Bean有5个作用域，但是最近有需求在多个地方获取同一个Bean，只修改其中一个参数。那么Bean只在一个地方注入，修改属性就好，名字能一样吗？一系列问题  
这个StepScope是什么鬼，5个默认的Scope能满足需求吗？  
<!--more--> 
##### [Bean scopes](https://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch04s04.html)

| Scope	| Description |
| -------- | ----- |
| singleton | Scopes a single bean definition to a single object instance per Spring IoC container. |
| prototype| Scopes a single bean definition to any number of object instances. |
| request | Scopes a single bean definition to the lifecycle of a single HTTP request; that is each and every HTTP request will have its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring ApplicationContext. |
| session | Scopes a single bean definition to the lifecycle of a HTTP Session. Only valid in the context of a web-aware Spring ApplicationContext. | 
| global session|Scopes a single bean definition to the lifecycle of a global HTTP Session. Typically only valid when used in a portlet context. Only valid in the context of a web-aware Spring ApplicationContext. |

通常我们使用的是singleton，默认也是。在测试中发现提供相同类型的Bean，需要指定不同的name标识（@Bean("three")），使用时对应@Qualifier("three")。  
##### 疑问：经测试发现提供相同类型Bean，不指定Qualifier会出错，但是在SpringBatch中指定了@StepScope却没报错，那是如何来区分应该在哪个Step中有效的呢？  
通过查看[StepScope文档](https://docs.spring.io/spring-batch/4.0.x/api/org/springframework/batch/core/configuration/annotation/StepScope.html)可以发现应该还有自定义Scope的存在来实现个性化需求，见[Bean scopes](https://docs.spring.io/spring/docs/3.0.0.M3/reference/html/ch04s04.html)之4.4.5 Custom scopes，下篇文章再记录研究Custom scopes

#### 参考   
[Spring中Bean的作用域、生命周期](https://www.cnblogs.com/zhanglei93/p/6231882.html)  
[Spring中Bean的五个作用域](http://blog.csdn.net/u011468990/article/details/49995865)  

###### 转载请标明出处： 
###### http://2tu.ghithub.io/2018/01/12/spring-bean-scope/ 
###### 本文出自Tu's blog