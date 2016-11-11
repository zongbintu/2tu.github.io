title: TV Apps Checklist
date: 2016-02-29 17:00:12
tags:
---
最近开发一款定制运行于Letv的 TV应用（流云TV），主要用于数据报表展示及小部份交互。本文主要参考于电视猫、VST两款应用，适配小米电视及乐视。
  
#### SDK版本及分辨率  
国内目前的电视都是从4.x的Phone SDK修改，所以我们的适配如下表

| 应用        | minSdkVersion           | targetVersion  | 分辨率  |
| ----- |:-------------:| :-----:| :-----:|
| 电视猫 | 8       |   16  |1280\*720|
| VST   | 15      |   19  |1920\*1080|
| <font color=red>流云TV</font> | <font color=red>14</font>      |   <font color=red>19</font>  |<font color=red>1920\*1080</font>(xxhdpi)|

<!--more-->  

#### 焦点状态显示  
电视端因为缺少触摸操作，焦点处理尤为重要。normal、focus、press状态设计上要明显。  
#### 其它  
剩下的就是按键处理、内容显示设计及是否显示屏保等。    
Manifest  
`<uses-feature
        android:name="android.software.leanback" android:required="true" />`
`<uses-feature
        android:name="android.hardware.type.television"
        android:required="true" />`
        
`<category android:name="android.intent.category.LEANBACK_LAUNCHER" />`  


参考：  
[小米应用开发者文档](http://dev.xiaomi.com/doc/p=2212/index.html)  
[乐视遥控器按键对应表](http://bbs.le.com/thread-838866-1.html)  
[Building Apps for TV](http://developer.android.com/intl/zh-cn/training/tv/index.html)
