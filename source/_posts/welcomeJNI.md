title: 使用AndroidStudio开发JNI并开启Native Debugger
date: 2016-03-24 22:09:37  
categories: Android  
tags: [JNI,AndroidStudio,Debugger,Native,C++]
---
不使用eclipse开发JNI，使用谷歌亲儿子AndroidStudio，并开启Native Debugger  
### 一、创建JNI工程
#### 1、android.useDeprecatedNdk=true  
在gradle.properties中添加“android.useDeprecatedNdk=true”，否则会报
”Error: NDK integration is deprecated in the current plugin.  Consider trying the new experimental plugin.“  
#### 2、创建CLog类
添加native方法  
`public static native void i();`  
#### 3、采用javah使用生成头文件  
创建JNI目录

<!--more-->

![](welcomeJNI01.webp)
创建后实际的jni目录为app/src/main/jni，与java同级  
打开Terminal,执行javah -d ../jni/ -jni com.tu.jnilog.clog.CLog命令生成头文件至jni目录  
#### 4、实现头文件  

```#include "com_tu_jnilog_clog_CLog.h"  
#include <android/log.h>  
#define LOG_TAG "CLOG"  
#define LOGI(...)    
__android_log_print(ANDROID_LOG_INFO, LOG_TAG,  __VA_ARGS__)

void Java_com_tu_jnilog_clog_CLog_i
        (JNIEnv *env, jclass){
        LOGI("c++ call android log api");
}  
```
#### 5、添加moduleName及支持平台  
接下来将c编译成so，引用并调用。  
在AndroidStudio中在defaultConfig配置中添加  
```ndk{
moduleName "CLog"  
ldLibs "log"//实现__android_log_print  
abiFilters "armeabi", "armeabi-v7a", "x86"//如果不添加，测试发现是全部平台  
}
```
#### 6、loadLibrary  
在CLog类中添加
```	static{
		System.loadLibrary("CLog");//与指定的module
	}
	```


### 二、开启Native Debugger  
#### 1、激活Native代码调试  
在module下build.gradle中buildTypes添加
```    debug{
      jniDebuggable true
    }
```  
否则会报Error: Build type isn's JNI debuggable
#### 2、Edit Configurations  
 Run->Edit Configurations->开启Hybrid调试
 ![](welcomeJNI02.webp)
 
打上断点，选择创建的Native运行即可

示例工程参见 [JNILog](https://github.com/2tu/JNILog)

参考：  
[Android Studio 1.5+ 中混合调试](http://toutiao.com/a6265272977680924930)