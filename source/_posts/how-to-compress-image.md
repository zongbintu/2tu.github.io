title: 一个万能算法压缩所有图片？  
date: 2017-06-06 17:17:36
tags: [Android,How compress,TingPNG,libjpeg,libjpeg-turbo,Huffman,Arithmetic,jpeg]
---
图片越来越多，相素越来越高，导致越来越大的图片必然绕不开压缩。  
作为基础功能的图片压缩，本身问题却很多。如：  
* android压缩下来比iOS大，质量却低  
* h5压缩图片导致崩溃  
* 微信号每天上传图片限制10w张  
* 压缩后图片变形、不清晰  
各路大神提供了诸多解决方案，  
解决质量速度问题的[libjpeg-turbo](https://github.com/libjpeg-turbo/libjpeg-turbo)，android7.0之后也采用该库；  
解决裁剪问题的[uCrop](https://github.com/Yalantis/uCrop)；  
压缩策略[Luban](https://github.com/Curzibn/Luban)；  
压缩质量、裁剪技术方案都是无需质疑选型使用就好。那么压缩策略呢？  
**Luban（鲁班） —— Android图片压缩工具，仿微信朋友圈压缩策略。**Luban可能是最接近微信朋友圈的图片压缩算法，有微信强大的用户量背书，我们是否可以在项目中直接使用该策略呢？  
答案是NO  
压缩策略其实与业务需求关系非常紧密。比如聊天时、展示文章等这个算法都特别合适，一般情况下该算法压缩出来的图片不大且可以被用户清晰辨识，如果看不清楚，可以点击下载查看原图。但是如果是拍证件，如身份证、银行卡这种就不需要。  
废话太多，写不下去了。<font color="red">**总之，没有万能算法，要做的是分析自己的业务应用场景。**</font>
