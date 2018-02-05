title: Activity调用外部为WebView设值不成功问题解决
date: 2017-12-15 19:41:34  
categories: Android  
tags: [RestoreInstance,WebView,Activity,onActivityForResult,onPageFinished]
---
简单的Android保存恢复数据，有很多app没有做到。如之前提到的知名APP[掘金v3.1.1](https://juejin.im/)，以及朋友公司的[书香云集v5.40.1](http://www.sxyj.net/)，基本每去一家公司都会要为公司产品检查修改此类问题。本以为自己是这方面的老司机，没想到最近同事反馈上来又有此类问题发生，而这次的问题却是与我有关。  
16年9月开始负责重构马上金融2.0，其中一项重要功能是hybrid资料表单以适应风控频繁变化的需求。采用jsbridge动态写入js解决js与native相互调用的安全性，此次按下不表。<font color="red">**问题发生在表单中js调用android访问联系人，获取联系人数据调用js设置时不成功**</font>  

##### 原因  
还原代码  
MainActivity  implements RestoreActivityResultCallback   
<!--more-->

```
  @Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
	.
	.
	.
    webView.setWebViewClient(new WebViewClient() {

      @Override public void onPageFinished(WebView view, String url) {
        super.onPageFinished(view, url);
        webView.onPageFinished(view, url);
      }
    });
    webView.addJavascriptInterface(new JSInteraction(), "contact");

    if (savedInstanceState != null) {
      webView.restoreState(savedInstanceState);
    } else {
      webView.loadUrl(url);
    }
  }
  
  @Override protected void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    webView.saveState(outState);
  }

  @Override protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_CODE_PICK_CONTACT) {
      webView.onActivityResult(requestCode, resultCode, data);
      if (Activity.RESULT_OK == resultCode && data != null) {
        setContact(data);
      }
    }
  }

  @Override public void restoreActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_CODE_PICK_CONTACT && RESULT_OK == resultCode) {
      setContact(data);
    }
  }

  private void setContact(Intent data) {
    String contact = readContactFormResult(data);
    webView.loadUrl("javascript:setContact(\"" + contact + "\")");
  }
```

Activity WebView restore流程  
![Activity WebView restore](/css/images/20171219_activity_webview_restore.jpeg)  
由于onActivityResult执行webView.loadUrl之前WebView并未restore完成，所以js执行不会生效  

##### 解决办法  
1、自定义WebView缓存onActivityResult结果  
2、WebViewClient.onPageFinished时回调WebView，由WebView将缓存的Activity Result回调至Activity中，并清空缓存Result数据  
3、Activity判断是否需要对WebView操作  
具体流程如下：  
![webview-restore](/css/images/20171219_activity_webview_restore_fixed.jpeg)   

**源码，请参见[webview-restore](https://github.com/2tu/webview-restore)**

##### 相关导读  
[android数据共享，Application\Singleton存储数据出错的问题](/2016/05/14/android-data-store/)
[WebView兼容问题之can not support WebGL](/2017/12/18/WebView-can-not-support-WebGL/)