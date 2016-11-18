title: android数据共享，Application\Singleton存储数据出错的问题
date: 2016-05-14 21:17:48
tags: android storage applicaton crash NullPointException
---
两天终于完成了某模块的数据共享问题的改造。此次问题就是传说中万恶的Application及Sigleton存数据的问题。   
曾经有大拿老张疑惑说Android怎么可能没有一个安全的临时存储数据的地方。接下来我们看一下在本次应用中实际发生的问题。并查看解决[知名app掘金v3.1.1](http://gold.xitu.io/app)出现的这个问题  
采用官方提供api机制来解决问题。

##### 一、问题  
应用切换到后台返回程序Crash或数据丢失。甚至在小内存手机中调用系统相机再回来应用就已经重启了
   
##### 二、解决办法
采用Intent及setArgments的方式来传值，采用onSaveInstanceState保存数据以供恢复。
###### Activity中Fragment恢复
<!--more-->
除了界面中传值及数据恢复外还涉及到界面的恢复。各View都由系统保存并恢复，但是在涉及到Fragment时，不会为我们直接将Fragment恢复到我们的Activity中，需要我们使用getFragmentManager().findFragmentByxxx获取后再add或replace上去。  

采用以上的方法基本可以解决问题，能使用户走正常流程。但是会发现数据有些卡顿，甚至会偶现不能正常恢复的情况。追求极致的程序猿又要开始折腾了。

##### 三、优化
###### 1、减小数据
保持数据短小轻快，这样可以更快启动下一界面及恢复
###### 2、本地缓存
如果数据量确实大不能保持精练，则采用Singleton加本地缓存（若对象不存在则从本地读取）。这样可以保证快速启动界面，然后再加载数据。  
本地缓存视数据大小及结构选择存储方式。可以简单粗暴的采用[Preference](https://github.com/2tu/fit)、ObjectFile，或者数据库、LruCache等.

##### 四、诱因及说明【没有耐心直接略过，真心废话】
Home回到桌面或者调用其它应用，我们的应用转入后台后降低优先级被回收（整个Application会被会关掉，所有变量数据都会被清空）。但是，切回应用Android的恢复机制却是重新启动Application，不进入LaunchActivity，而是直接恢复到最近的界面。  
这个时候问题来了，若正常重新启动应用程序，走正常流程，数据重新生成没有问题。但是直接进入最近界面之前生成的数据已经丢失了，此时直接引用数据将会引起Crash。  
这时看官们就说了，直接加个判断是否为Null不就可以了。但数据为Null，用户不能走正常流程，这一定不是我们想要的。那么有没有一种机制解决这个问题呢，答应是有的。Android提供了onSaveInstanceState来保存数据，恢复时可以从savedInstanceState中获取数据。  
 OK，讲到这里方案已经呼之欲出了，答案是请看本文前面的内容。   
 

##### 五、实战分析——[掘金v3.1.1](http://gold.xitu.io/app)
![文章详情](/css/images/20160514_onSaveInstanceState_xitu_error.webp)  
进入掘金文章详情页切换到后台再恢复时会出现“文章加载失败”的提示也是这个问题导致。  
WebViewEntryActivity(文章详情)
我们分析一下代码，文章详情中的url由Entry对象v中从获取（LeanCloud）  

首先来看加载失败的代码

```
  private void q() {
    if (v == null) aj.a(this, "文章信息加载失败");
    this.editorName.setText(getIntent().getStringExtra("name"));
    g.a(this).a(getIntent().getStringExtra("avatar")).a(this.avatar);
    return;
    this.commentCount.setText(Entry.getString(v.getCommentCount()));
    b.a(v, new ff(this));
    if ((!(k.b(v.getUrl()))) || (v.getTags() == null) || (v.getTags().size() <= 0)) break;
    com.daimajia.gold.c.z.a(v.getTags(), new fg(this));
    u();
  }
```

v的获取方式有两个地方，一个是在方法a中传入，另外一个为EventBus调用onEventMainThread时传入。  
我们切回掘金文章详情页时，WebViewEntryActivity生命周期会重新执行一次，经过查找引用的地方，我们确定方法a不会被执行。EventBus处为ShareEntryActivity调用，显然这个方法也没法执行。v就未能被初始化，为null，就必然提示“文章信息加载失败”

```
  public static void a(Entry paramEntry)
  {
    v = paramEntry;
  }
```

```
  public void onEventMainThread(f paramf)
  {
    if ((v != null) && (v.getUrl().equals(paramf.a.getUrl())))
      v = paramf.a;
  }
```

具体解决办法见上文。但是对于View的状态其实Android自动作了处理，这里因为是Webview才出现这个问题，那要么解决呢？

```
  @Override protected void onSaveInstanceState(Bundle paramBundle) {
    super.onSaveInstanceState(paramBundle);
    mWebView.saveState(paramBundle);
  }
```

```
  public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    if (savedInstanceState != null) {
      mWebView.restoreState(savedInstanceState);
    } else {
      mWebView.loadUrl("http://2tu.github.io/2016/05/14/android-data-store/");
    }
  }
```

##### 五、PS
###### 通过 开发者选项->不保留活动 制造该现象
###### 采用官方推荐方式开发


