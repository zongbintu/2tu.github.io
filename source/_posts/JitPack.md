title: Gradle 2.4之后使用JitPack发布Github开源库
date: 2015-11-16 21:35:16
tags:
---
之前采用过jcenter发布，各种步骤非常麻烦。在“掘金”上看到一篇文章可采用JitPack一键发布，照着抄遇到了一点坑。待此记录。  
原作者的文章地址“http://www.dss886.com/android/2015/10/17/16-23/”

##### 一、配置并提交到Github
Gradle  
To enable installing into local maven repository and JitPack you need to add the android-maven plugin.

If using Gradle 2.4 or later:  
Gradle 2.4之后要添加如下配置

###### 1) In your root build.gradle:
root/build.gradle添加
```
buildscript {   
  dependencies {  
      classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0' // Add this line
```
<!--more--> 
###### 2) Add the following lines to your library/build.gradle:
library/build.gradle添加
```
apply plugin: 'com.github.dcendents.android-maven'   
group='com.github.YourUsername' 
 ```
 
##### 二、获取JitPack中依赖地址  
打开https://jitpack.io/  
输入Github地址  
![](/css/images/20151106_JitPack_lookup.webp)  
点击“Look up”  
点击“Get it”  
查看Log

###### 1）在项目中引用  
root/build.gradle  
```
allprojects {  
    repositories {  
        jcenter()  
        maven { url "https://jitpack.io" }  
   }  
}
```

###### 2）添加依赖  
```
dependencies {  
    compile 'com.github.username:Project-name:v1.0'
}
```  
 
 一切OK。项目中也可以引用，不用再引用AAR。
 
 目前（2016-8-31）JitPack默认采用gradle2.7，可以采用wrapper方式使用指定Gradle版本。   
 另外也支持module，详情见下方参考文档。
 
 参考：https://jitpack.io/docs/ANDROID/  
 https://jitpack.io/docs/BUILDING/#multi-module-projects   
 http://www.dss886.com/android/2015/10/17/16-23/