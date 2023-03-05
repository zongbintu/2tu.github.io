title: 不动态创建链接情况下在Firebase Dynamic Link获取动态参数
tags: [Dynamic Links,动态参数]
date: 2023-03-05 09:26:30
categories: Android
---

在使用Firebase的Dynamic Links做运营时，需要在链接中带去不同的参数，APP做不同的响应。如，在链接中带商品ID，用户点击不同链接进入不同商品详情页。Firebase中有两种方法可以实现。
1、创建动态链接
[创建动态链接-简介](https://firebase.google.com/docs/dynamic-links/create-links?hl=zh-cn)
2、通过[动态链接参数](https://firebase.google.com/docs/dynamic-links/create-manually?hl=zh-cn#parameters)来实现不创建动态链接APP接收自定义参数目的。缺点就是短链接变成了长链接。
下面看看做实验的demo
这是之前建立的链接
![链接详情](deeplink_detail.png)
<!--more-->
我们希望在APP中获取参数，如
title=How to custom parmeter the Dynamic Link
articleLink=https://console.firebase.google.com/project/test-c67b6/durablelinks/details/https:~2F~2Fcustomparmeter.page.link~2F6SuK
那我们需要在链接中给link添加想要加的参数
![自定义参数规则](link_parameter.png)

效果
链接：
https://customparmeter.page.link/?link=https://blog.520wa.com?title=How to custom parmeter the Dynamic Link%26articleLink%3Dhttps%3A%2F%2Fconsole.firebase.google.com%2Fproject%2Ftest-c67b6%2Fdurablelinks%2Fdetails%2Fhttps%3A~2F~2Fcustomparmeter.page.link~2F6SuK&apn=com.example.deeplink
APP中获取结果：
```
2023-03-04 22:57:32.532 12747-12747/com.example.deeplink W/com.example.deeplink.ComicActivity: getDynamicLink:onSuccess:https://blog.520wa.com?title=How+to+custom+parmeter+the+Dynamic+Link&articleLink=https://console.firebase.google.com/project/test-c67b6/durablelinks/details/https:~2F~2Fcustomparmeter.page.link~2F6SuK
```

注：link后的参数需要通过URLEncode编码，否则不能正确获取参数。不能是另一个动态链接

