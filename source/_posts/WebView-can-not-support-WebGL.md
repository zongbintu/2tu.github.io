title: WebView兼容问题之can not support WebGL
date: 2017-12-18 16:42:06  
categories: Android  
tags: [WebGL,WebView,兼容问题,experimental-webgl,WEBGL_debug_renderer_info,WebView,v36,chrome]
---
[合作方页面](https://youjie.kongapi.com/?channel=xiangduoduoylpyj&utm_source=daikuan&utm_medium=xiangduoduoylpyj#/loan-in-index)能在浏览器中打开，不能在我们的WebView中打开，运营打上门来。  
作为一个有态度的码农当然是要怒回去的。于是调试看了对方h5报错如下：
<!--more-->

```
VM122:158 NativeBridge Ready  
app.e35363b….js:1 Uncaught TypeError: Cannot read property 'getExtension' of null
    at g (app.e35363b….js:1)
    at Object.<anonymous> (app.e35363b….js:1)
    at Object.<anonymous> (app.e35363b….js:1)
    at r (manifest.d23aea9….js:1)
    at Object.<anonymous> (app.e35363b….js:1)
    at Object.<anonymous> (app.e35363b….js:1)
    at r (manifest.d23aea9….js:1)
    at Object.<anonymous> (app.e35363b….js:1)
    at r (manifest.d23aea9….js:1)
    at Object.<anonymous> (app.e35363b….js:1)
g @ app.e35363b….js:1
(anonymous) @ app.e35363b….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
r @ manifest.d23aea9….js:1
window.webpackJsonp @ manifest.d23aea9….js:1
(anonymous) @ app.e35363b….js:1
```

我们观摩对方代码

```  
e=t.getContext("experimental-webgl"),
n=e.getExtension("WEBGL_debug_renderer_info");
return n?e.getParameter(n.UNMASKED_RENDERER_WEBGL):
"Apple unknown GPU"
```  

H5中采用了WebGL，android WebView v36才开始支持WebGL，所以出现了这样的兼容性问题。  
运营再怒，在你们的App中出现这个问题，但是就在该手机的浏览器、微信中都是可以打开的。  
有态度的码农知道这次有口莫辩了。如果对方用的Chrome，那问题一定是同步的，但是其它浏览器自研内核，有的是支持的。  
于是这时候我们应该怎样微笑着玩下去，代码截图，解决方案截图与合作方的工程师相爱相杀去了。  

##### 相关导读  
[Activity调用外部为WebView设值不成功问题解决](/2017/12/15/WebView-RestoreInstance-onActivityForResult-not-work/)
[301 造成WebView 不能正常访问](/2017/12/07/fix-301-redirect/)
[WebView restore](/2016/05/14/android-data-store/)

##### 参考  
[WebView for Android](https://developer.chrome.com/multidevice/webview/overview)  
[enabling-webgl-support-for-android-webview](https://stackoverflow.com/questions/15395245/enabling-webgl-support-for-android-webview)