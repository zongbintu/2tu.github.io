title: Java类文件在JVM运行的生命周期
tags: [Interview,JVM]
date: 2024-02-27 19:30:08
categories: Java
---
#### java Class文件结构
Java .class 文件是 Java 编程语言的关键组件，遵循精确且定义的结构。 这种结构不仅对于 Java 虚拟机 (JVM) 正确加载和执行字节码至关重要，而且还提供了有关编译后的 Java 代码的大量信息。 下面，我们深入研究类文件结构的基本元素，详细说明每个组件及其在整体架构中的重要性。
##### class文件格式
类文件由单个 ClassFile 结构组成。 该结构由 JVM 规范定义并遵循特定格式，其中包括以下主要部分：
- Magic Number魔数：固定值 (0xCAFEBABE)。 此唯一标识符验证该文件是否是 JVM 可读的有效类文件。
- Version Information版本号：major_version、minor_version，java版本号
- Constant Pool常量池
- Access Flags访问标识
- This Class, Super Class,and Interfaces类索引、父类索引与接口索引集合
- Fields字段集合
- Methods方法表集合
- Attributes属性表集合

以上信息如何查看？
```
javap [options] classes...
```

JVM需要使用上述信息来正确加载、验证和执行

其是常量池，它是一个集中的字典，经常被类文件中的其他部分引用，突出了它在整个架构中的重要性。

#### Class文件在JVM中的生命周期
![The Class file Lifecycle of a Java Application](class-file-lifecycle-in-jvm.png)
##### 1. Loading加载
类加载过程执行以下三个功能：
从clas文件创建二进制数据流
根据内部数据结构解析二进制数据
创建 java.lang.Class 的实例
完成此操作后，类实例就可以进行链接了。
##### 2. Linking链接
###### 2.1 Verification验证
此步骤可确保安全性和完整性。JVM验证class文件的正确性，文件格式验证、语法是否有效、是否符合Java语言规范。
###### 2.2 Preparation准备
在准备过程中，JVM 会为类静态变量分配内存，并将其初始化为默认值。
###### 2.3 Resolution解析
解析阶段包括将类文件中的符号引用解析为直接引用。这就是 JVM 常量池发挥关键作用的地方。主要针对类或接口、字段、类方法、方法类型等。
##### 3. Initializes初始化
执行静态块： 这一阶段涉及执行静态初始化程序和静态块。JVM 会初始化静态字段，并按照它们在类文件中出现的顺序执行任何静态初始化块。
设置最终值： 为类的最终变量分配值，这些值在类的生命周期内不可更改。
##### 4. Usage使用
实例化： JVM 根据应用程序的需要创建类的实例。
执行： 根据运行程序的要求调用和执行方法，访问字段。JVM 会将字节码解释或即时编译为机器代码以便执行。
##### 5. Unloading卸载
垃圾回收： 当一个类不再需要，也没有对其实例的实时引用时，它就可以被卸载。JVM 的垃圾回收器会回收分配给类的内存。



[The Anatomy of a Java Virtual Machine Class File](https://medium.com/@AlexanderObregon/the-anatomy-of-a-java-virtual-machine-class-file-e3773d23f3e3)
[The Execution Lifecycle of a Java Application](https://www.cesarsotovalero.net/blog/how-the-jvm-executes-java-code.html)
[javap](https://docs.oracle.com/en/java/javase/11/tools/javap.html)
[Chapter 4. The class File Format](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html)
