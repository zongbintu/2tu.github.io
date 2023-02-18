title: Android中添加背景水印，含WebView  
tags: [水印,watermark,WebView水印,网页水印]  
date: 2020-04-19 10:59:46
categories: Android
---

绝密资料，必须加上水印背景以免外传，在Android中如何实现呢？分为普通背景水印和加载网页中添加水印。当然网页本身可以直接加，WebView加载下来直接就有了，这种方案我们不讨论，只讨论在WebView中追加。  
先上效果图：  
普通背景水印 
![普通背景水印](/css/images/watermark_bg.jpeg)  
网页水印  
<!--more-->
![网页水印](/css/images/watermark_webview.jpeg)

####  普通背景水印
创建一个水印Bitmap，为rootView设置背景。so easy,直接上代码 
 
```
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_watermark);
    if(Build.VERSION.SDK_INT>=Build.VERSION_CODES.JELLY_BEAN) {
      findViewById(android.R.id.content).setBackground(background("水印"));
    }else{
      findViewById(android.R.id.content).setBackgroundDrawable(background("水印"));
    }
  }
  BitmapDrawable background(String watermark){
    Bitmap bitmap = Bitmap.createBitmap(240, 240, Bitmap.Config.ARGB_8888);
    Canvas canvas = new Canvas(bitmap);
    canvas.drawColor(Color.WHITE);
    Paint paint = new Paint();
    paint.setColor(Color.GRAY);
    paint.setAlpha(80);
    paint.setAntiAlias(true);
    paint.setTextAlign(Paint.Align.LEFT);
    paint.setTextSize(40);
    Path path = new Path();
    path.moveTo(30, 150);
    path.lineTo(300, 0);
    canvas.drawTextOnPath(watermark, path, 0, 30, paint);

    BitmapDrawable bitmapDrawable = new BitmapDrawable(bitmap);
    bitmapDrawable.setTileModeXY(Shader.TileMode.REPEAT, Shader.TileMode.REPEAT);
    bitmapDrawable.setDither(true);
    return  bitmapDrawable;
  }
```

####  网页水印
网页背景水印  
在网页加载完成后插入javascript添加div水印  
```
String addWaterMarker(String watermark) {
    String js = "var newscript = document.createElement(\"script\");";
    js += "var bbTextNode = document.createTextNode(\"";
    js += "var can = document.createElement('canvas');";
    js +=
        "var body = document.body;body.appendChild(can);can.width=400; can.height=200;can.style.display='none';"
            + "var cans = can.getContext('2d');cans.rotate(-20*Math.PI/180);cans.font = '16px Microsoft JhengHei';"
            + "cans.fillStyle = 'rgba(17, 17, 17, 0.50)';cans.textAlign = 'left';"
            + "cans.textBaseline = 'Middle';cans.fillText('watermark',can.width/3,can.height/2);"
            + "body.style.backgroundImage='url('+can.toDataURL('image/png')+')';"
    ;
    js += "\");";
    js += "newscript.appendChild(bbTextNode);";
    js += "var t = document.createTextNode(\"alert('action');\");";
    js += "newscript.appendChild(t);";
    //js += "newscript.onload=function(){xxx();};";  //xxx()代表js中某方法
    js += "document.body.appendChild(newscript);";

    return js;
  }

  String addWaterMarkerDiv(String watermark) {
    String js = "var newscript = document.createElement(\"script\");";

    js += "var waterMarkDiv = document.createTextNode(\""
        //+"if(document.getElementById('waterMark') != null) return;"
        + "var m = 'waterMark';"
        + "var newMark = document.createElement('div');"
        + "newMark.id = m;"
        + "newMark.style.position = 'absolute';"
        + "newMark.style.zIndex = '9527';"
        + "newMark.style.top = '0px';"
        + "newMark.style.left = '0px';"
        + "newMark.style.width = '800px';"
        + "newMark.style.height = '400px';"
        + "alert('newmark');"
        + "\");"
    ;

    js += "newscript.appendChild(waterMarkDiv);";
    js += "var bbTextNode = document.createTextNode(\"";
    js += "var can = document.createElement('canvas');";
    js +=
        "var body = document.body;body.appendChild(can);can.width=400; can.height=200;can.style.display='none';"
            + "var cans = can.getContext('2d');cans.rotate(-20*Math.PI/180);cans.font = '16px Microsoft JhengHei';"
            + "cans.fillStyle = 'rgba(17, 17, 17, 0.50)';cans.textAlign = 'left';"
            + "cans.textBaseline = 'Middle';cans.fillText('watermark',can.width/3,can.height/2);"
            + "newMark.style.backgroundImage='url('+can.toDataURL('image/png')+')';"
            + "newMark.style.filter = 'alpha(opacity=50)';"
            + "document.body.appendChild(newMark);"
    ;
    js += "\");";
    js += "newscript.appendChild(bbTextNode);";
    js += "document.body.appendChild(newscript);";

    return js;
  }
```

[完整demo例子](https://github.com/2tu/Demos/tree/master/app/src/main/java/com/tu/example/watermark)


##### 引用 
[【Android】webview javascript 注入方法](https://www.cnblogs.com/rayray/p/3680500.html)  
[Javascript 网页水印(非图片水印)实现代码](https://m.jb51.net/article/22279.htm)  
[html如何添加水印？](https://segmentfault.com/q/1010000011817297) 