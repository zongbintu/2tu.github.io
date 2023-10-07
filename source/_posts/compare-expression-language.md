title: 表达式引擎预研  
tags: [Aviator]  
date: 2022-09-02 22:22:39  
categories: Java
---

### 背景
最后业务有需求需要动态计算表达式，基于公式对某些值进行再加工计算。所以对表达式引擎进行一个简单的了解。
表达式引擎就是为了表达式的动态求值计算。
通常对于各个类库对比，我们需要看厂商、License、社区活跃度、优缺点等来判断其符合的使用场景，最终决定应该用哪个。
由于时间较紧，只做简单收集，在项目中本已使用AviatorScript的情况下就选择了它。

### 特性对比
|  | [AviatorScript](https://github.com/killme2008/aviatorscript) | [Groovy](http://www.groovy-lang.org/) | [SpEL](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#expressions) |  [QLExpress](https://github.com/alibaba/QLExpress) |
|--------|--------|
|    Version    | 5.3.2 | 4.0.4 | 5.3.22 |  3.3.0      |
|    厂商    | killme2008 | Apache | Spring | alibaba |
|    License    | GPL V3 | Apache 2 License | Apache License 2.0 |  Apache License 2.0 |
|    Language | 基于Java | 基于Java | Java | Java |
|    优点    |   高性能<br> 轻量级    | Flat learning curve<br>Powerful features<br>Smooth Java integration<br>Domain-Specific Languages<br>Vibrant and rich ecosystem<br>Scripting and testing glue |        |   线程安全<br>高性能<br>弱类型脚本语言<br>安全控制<br>代码精简     |


还有[OGNL](https://commons.apache.org/proper/commons-ognl/language-guide.html)、[Drools](https://www.drools.org/)、[EasyRules](https://github.com/j-easy/easy-rules)

另外[MVEL](https://github.com/mvel/mvel)、[IKExpression](https://code.google.com/p/ik-expression/)很久未更新，不列


