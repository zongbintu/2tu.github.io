title: gradle查看解决依赖问题
date: 2017-05-11 17:47:32
tags: transformClassesWithJarMerging duplicate entry TransformException gradle dependencies
---
相信每个人都遇到过重复引用库、冲突的问题。如support-v4、appcompat-v7、NineOldAndroids很容易被多次引用。  
前同事在临上线前打包遇到mergb出错（内心飘过一万头草泥马）  
![TransformException](/css/images/20170512_duplicate.jpeg)  
How fix?
<!--more-->  
一个一个module检查过去没有，这时候就有可能是第三方库引用了，可是第三方库那么多，How do?头疼，先换个姿势找到哪两个jar导致重复。通过打开文件发现DisplayManagerCompat类属于哪两个jar包    
![Open class](/css/images/20170512_multi.jpeg)  
![jar](/css/images/20170512_multi_class.jpeg)  
可以确定出问题的jar为support-compat-25.1.0和support-v4-22.2.0，我们自己有依赖compat、v4 22.2.0，现在要找出冲突的compat25.1是哪个库添加的（前戏太长，我们要直奔主题）。使用maven时可以查看依赖关系，没理由要取代maven的gradle不支持。  
- gradle projects
先查看有哪些项目  
![projects](/css/images/20170512_projects.jpeg)   
- gradle dependencies  
![dependencies](/css/images/20170512_dependencies.jpeg)  
从图中可以看到是butterknife8.5.1依赖了compat25.1.0。  
- 添加exclude  
 修改compile添加exclude后解决
 ```
   compile ("com.jakewharton:butterknife:8.5.1") {
        exclude group : 'com.android.support'
    }
 ```

#### 扩展  
- 强制指定低版本force  
同一个grop的库多个引用时，会自动取最版本最高的，即我们通过gradle dependencies查看后的标(*)或者 -> 后的版本。  
但是我们有时候需要覆盖使用为低版本，应用场景，使用React Native时想指定‘com.android.support:appcompat-v7:23.0.1’可以修改为  
```
   compile ("com.android.support:appcompat-v7:23.0.1") {
        force = true
    }
```

- 查看pom依赖关系  
可以通过gradle命令查看依赖关系，那么我们写在build.gradle里面的依赖关系一定也有地方指定。每个工程里面指定gradle，gradle查找依赖库。根据这个逻辑，查看项目文件夹下的.idea->libraries->butterknife_8_5_1.xml、support_compat_25_1_0.xml、support_v4_22_2_0.xml，其中butterknife_8_5_1.xml内容如下：
```xml
<component name="libraryTable">
  <library name="butterknife-8.5.1">
    <CLASSES>
      <root url="file://$PROJECT_DIR$/regClientDoc/build/intermediates/exploded-aar/com.jakewharton/butterknife/8.5.1/res" />
      <root url="jar://$PROJECT_DIR$/regClientDoc/build/intermediates/exploded-aar/com.jakewharton/butterknife/8.5.1/jars/classes.jar!/" />
    </CLASSES>
    <JAVADOC />
    <SOURCES>
      <root url="jar://$USER_HOME$/.gradle/caches/modules-2/files-2.1/com.jakewharton/butterknife/8.5.1/16724f486072e3ab970fc47ccff6f9013061fa6/butterknife-8.5.1-sources.jar!/" />
    </SOURCES>
  </library>
</component>
```

根据source文件上级目录进入，找到butterknife-8.5.1.pom文件，内容如下： 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.jakewharton</groupId>
  <artifactId>butterknife</artifactId>
  <version>8.5.1</version>
  <packaging>aar</packaging>
  <name>Butterknife</name>
  <description>Field and method binding for Android views.</description>
  <url>https://github.com/JakeWharton/butterknife/</url>
  <licenses>
    <license>
      <name>The Apache Software License, Version 2.0</name>
      <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
      <distribution>repo</distribution>
    </license>
  </licenses>
  <developers>
    <developer>
      <id>jakewharton</id>
      <name>Jake Wharton</name>
    </developer>
  </developers>
  <scm>
    <connection>scm:git:git://github.com/JakeWharton/butterknife.git</connection>
    <developerConnection>scm:git:ssh://git@github.com/JakeWharton/butterknife.git</developerConnection>
    <url>https://github.com/JakeWharton/butterknife/</url>
  </scm>
  <dependencies>
    <dependency>
      <groupId>com.jakewharton</groupId>
      <artifactId>butterknife-annotations</artifactId>
      <version>8.5.1</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>com.android.support</groupId>
      <artifactId>support-annotations</artifactId>
      <version>25.1.0</version>
      <scope>compile</scope>
    </dependency>
    <!-- 罪魁祸首  -->
    <dependency>
      <groupId>com.android.support</groupId>
      <artifactId>support-compat</artifactId>
      <version>25.1.0</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
</project>

```
可以看到其中有依赖support-compat25.1.0    
- [exclude的其它用法](https://docs.gradle.org/current/userguide/dependency_management.html)  


#### 参考：
[Add Build Dependencies](https://developer.android.google.cn/studio/build/dependencies.html)   
[Dependency Management](https://docs.gradle.org/current/userguide/dependency_management.html)  
[Gradle Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) 