title: Jenkins构建Android、IOS并上传到蒲公英
date: 2015-12-23 16:22:00  
categories: 其它  
tags: [Jenkins,iOS]
---
#### 前言  
作为产品级的外包，常常会出现fix一个bug要更新十几甚至几十个产品（打包上传）。作为程序猿看到这种需求时，第一想法就是“是时候找办法偷懒了”。  
虽然Android以前采用Ant打包构建，现在与时俱进采用Gradle打包所有产品，然后再上传。  
IOS也可以采用命令行的方式做到。  
但是为了实现傻瓜式，可视化，同时兼容Android、IOS的方式，采用Jenkins实现很有必要。  

#### 实现步骤  
1、下载安装Jenkins  
2、配置Jenkins安装插件  
3、创建Job实现构建  
4、上传至蒲公英   
<!--more-->  
##### 我的环境  
Mac  
JDK 1.7  
Gradle 2.8  
IOS 8.0  
Jenkins 1.641   

##### 一、下载安装Jenkins  
在[Jenkins](http://jenkins-ci.org/)下载war包  
运行java -jar jenkins.war --httpPort=8888，提示"信息: Jenkins is fully up and running"，表示启动成功  
打开“http://localhost:8888/”

##### 二、配置Jenkins安装插件  
###### 1、安装插件  
系统管理-》插件管理-》可选插件-》过滤-》输入名称安装  
Git plugin  
Multiple SCMs plugin（针对多分支或多Git地址的情况，Git Plugin不能实现）  
Ftp plugin(FTP上传)  
Publish Over SSH(SFTP上传)  
Gradle plugin  
Xcode plugin  

###### 2、配置  
1）Gradle相关配置，添加GRADLE____HOME环境变量或  
系统管理-》系统设置-》Gradle安装，选择Gradle目录  
2）SFTP相关配置  
3）ANDROID____HOME环境变量添加，否则要报“SDK location not found”。或者为Android项目添加“local.properties”文件，我们当然选择配置环境变量

###### 3、创建JOB  
创建JOB的几个问题
1）Multiple SCMs时注意选择subdirectory  
2）Android 包Gradle
3)IOS 配置选择Target及Xcode Project File  
选择Pack application and build .ipa的时候，证书要选择正确。我在打包时出现首先匹配的是\*的证书。删除\*证书后就出现匹配不到证书的情况。最后解决办法是在Build Settings里面配置Release的证书，如图  
![](/css/images/20151223_Jenkins_IOS_Sign.webp)  
 
###### 4、上传至蒲公英  
 执行下列命令上传  
filen=\`find . -name *release*.apk\`;  
curl -F "file=@$filen" -F "uKey=4602339dac5705c81578cd8580505ab5" -F "_api_key=e3d6725adc24fce33c5210271ef1efed" http://www.pgyer.com/apiv1/app/upload
 ![](/css/images/20151223_ExecuteShell.webp)  
   
#### 后记  
上传到蒲公英只作为测试使用。正式分发产品时会更新到自己的网站上，并更新数据库，旧有的包备份等一套应用。目前采用的.net + php。发现并不能通用，后续希望改为shell + php
  
#### 参考  
http://www.itnose.net/detail/6075531.html  
http://blog.csdn.net/guojin08/article/details/39026795  
http://www.android100.org/html/201506/22/156680.html  
https://wiki.jenkins-ci.org/display/JENKINS/Publish+Over+SSH+Plugin  
