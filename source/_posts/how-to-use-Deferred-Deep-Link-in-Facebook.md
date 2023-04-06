title: Facebook广告中使用Deferred Deep Link
tags: [Defeered Deep Link]
date: 2023-04-06 22:57:42
categories: Android
---

在运营时，学习使用了App Link和Deep Link，之前也使用了Firebase中的Dynamic Links在用户未安装App时跳转至Google Play安装后再打开仍然能获取到链接内容，带用户到推荐的个性化页面。
后面由于运营需要在Facebook上做推广，需要接入Meta的[Deep Link](https://developers.facebook.com/docs/app-ads/deep-linking)。
运营时有3个场景，我们的App，以下简称A
1. 用户已安装A，点击Facebook广告链接，正常打开A并跳转至推广页面
2. 用户未安装A，点击Facebook广告链接，打开Google play store，下载安装，并在Google play store中点击打开，正常打开A并跳转至推广页面
3. 用户未安装A，点击Facebook广告链接，打开Google play store，下载安装，回到桌面点击A，正常打开A并跳转至推广页面
实现1，只需要实现Meta中的[Deep Link](https://developers.facebook.com/docs/app-ads/deep-linking)
实现2、3则需要在实现Deep Link的基础外加上Defeered Deep Link
<!-- more -->
![Prerequisites](DeferredDeepLinkRequired.png)
**注意**
``` Android
AppLinkData.fetchDeferredAppLinkData
```
需要在已启动的Activity中执行才能获得结果。
即
``` Android
        <activity
            android:name=".SplashActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name=".DeepLinkActivity"
            android:exported="true" >
            <intent-filter android:autoVerify="true">
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="https"
                    android:host="blog.520wa.com"/>

                <data
                    android:host="520wa"
                    android:scheme="blog"
                    android:pathPrefix="/lunch"
                    />
            </intent-filter>
        </activity>
```
在2、3的情况下，无法触达DeepLinkActivity，并执行DeepLinkActivity中的AppLinkData.fetchDeferredAppLinkData方法获取延迟深度链接。需要将AppLinkData.fetchDeferredAppLinkData放到SplashActivity中。
且AppLinkData.fetchDeferredAppLinkData只能调1次

参考
[App深度链接与延迟深度链接](https://www.biaodianfu.com/deep-link-deferred-deeplink.html)

