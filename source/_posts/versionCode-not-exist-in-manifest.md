title: Manifest中找不到versionCode?  
date: 2018-01-09 13:49:39  
categories: Android  
tags: [versionCode,manifest,APKParser,AXMLPrinter2]
---

#### 起因  
17年做的update（升级平台）交由其它团队维护后，发现解析Manifest后未找到versionCode、versionName信息。  
大家都讲终极绝招，使用aapt来获取，但是这样耦合低，并且得在服务器上维护Android环境，显然不合理。所以要干他  
此文都是解决思路，心急直接看最后的结论

#### 环境  
* Android Studio 3.0
* Gradle 4.1
* targetSdkVersion 26 minSdkVersion 8
* apktool 2.3.1
* AXMLPrinter2
* APKParser

<!--more-->

####  分析问题
#####  apktool  
第一想法，通过apktool反编译查看AndroidManifest中是否不存在version信息。  

```
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.tu.sample" platformBuildVersionCode="26" platformBuildVersionName="7.1.1">
```
结果是不存在的，于是把怀疑指向是否google的锅，Android Studio?Gradle?targetVersion?

#####  Android Studio?  
[Android Studio 3.0 Canary 2: attribute 'android:versionCode' not found](https://stackoverflow.com/questions/44225780/android-studio-3-0-canary-2-attribute-androidversioncode-not-found)  
然后经过对比发现仍然不是这个问题，继续搞  

#####  Gradle?targetVersion?  
安装在低版本的手机上仍然可以获取，即使Google要改这个规则至少得兼容旧版本。并且阅读Android官方文档后也没见有提及，所以这个原因也可以排除。见[设置应用版本信息](https://developer.android.google.cn/studio/publish/versioning.html?hl=zh-cn)  
程序猿要开始怀疑人生了

#####  Android Studio Analyze APK  
无意间使用Analyze APK查看了一下，信息如下

```
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.tu.sample"
    platformBuildVersionCode="26"
    platformBuildVersionName="7.1.1"
    android:versionCode="24400"
    android:versionName="2.4.4">
``` 
艹尼玛，居然是有的，因为官方解析，还是因为真的有？

#####  Build目录  
查看build/intermediates/manifests/full/debug和release下的AndroidManifest均与Analyze APK查看的内容匹配。  
开始怀疑自己

#####  apktool?
再看apktool解析出来的内容，其中有一个文件apktool.yml，简要内容如下

```
!!brut.androlib.meta.MetaInfo
apkFileName: sample.apk
compressionType: false
doNotCompress:
- arsc
isFrameworkApk: false
packageInfo:
  forcedPackageId: '127'
  renameManifestPackage: null
sdkInfo:
  minSdkVersion: '8'
  targetSdkVersion: '26'
usesFramework:
  ids:
  - 1
  tag: null
version: 3.0.1
versionInfo:
  versionCode: '24400'
  versionName: 2.4.4
```
居然也是有的，那么是解析方式变了，还是Manifest中没有？终极绝招读源码，但是由于我只是抽空（好奇）支持一把，没有时间耗（懒）。但总是舒了一口气，不用再使用aapt这种方式来解决了。  
回到我们自己的解析程序中来

#####  AXMLPrinter2
update中采用[AXMLPrinter2](https://code.google.com/archive/p/android4me/downloads)库来解析。这个库早已没维护并衍生了很多其它版本，如[APKParser](https://code.google.com/archive/p/xml-apk-parser/)  
发现通过这个库解析出来的Manifest是含有version信息的，内容与Analyze APK一致。

####  结论
Manifest中仍然有版本信息，规则不变。由于AXMLPrinter2解析库问题导致。具体原因不分析，留给目前的团队和有兴趣的人解决。

#### 参考  
[设置应用版本信息](https://developer.android.google.cn/studio/publish/versioning.html?hl=zh-cn)  
[AXMLPrinter2](https://code.google.com/archive/p/android4me/downloads)  
[APKParser](https://code.google.com/archive/p/xml-apk-parser/)  
[Android Studio 3.0 Canary 2: attribute 'android:versionCode' not found](https://stackoverflow.com/questions/44225780/android-studio-3-0-canary-2-attribute-androidversioncode-not-found)

###### 转载请标明出处： 
###### http://2tu.github.io/2018/01/09/versionCode-not-exist-in-manifest/ 
###### 本文出自Tu's blog