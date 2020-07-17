title: 浏览器打开图片链接时直接显示或下载到底怎么回事  
tags: [阿里云显示图片,图片链接,下载图片,浏览器显示图片,OSS, Content-Disposition: attachment;,image/jpeg,Content-Type]
date: 2020-07-17 23:49:36
categories: 前端
---

最近瞎JB搞了一通，各个项目都是喊组装上就交付，难以有时间学习总结，简直违反程序员的基本原则。很多时候已经忘记踩了哪些坑，如何解决的了。 
 
#### 问题
图片上传至阿里云OSS生成链接在浏览器中打开是直接下载文件，而需求需要打开图片而非下载。

####  常用方式

浏览器中的图片，通常印象中，前端上传文件获取url，终端当然是下载再展示。浏览器端都是img标签展示

```
<img src="/css/images/favicon.png"/>
```
下载图片及附件时都是右键保存或者点击按钮下载写入文件流中。  
最近的需求中提到上传的图片生成的url，在浏览器中是打开而非下载文件 。我们正常上传到我们服务器上的是没问题的。这个问题出在阿里云OSS上？到底要怎么玩？

#### HEADER  

就经验来讲肯定是响应头header的问题。Android有类似的机制，view-type啥的，你是直接在别的应用中打开，还是怎么操作。  
对比下载和打开图片两种不同链接  

<!-- more -->

##### 打开图片
链接：http://blog.520wa.com/css/images/watermark_bg.jpeg  
Response Headers

```
HTTP/1.1 200 OK
Server: nginx/1.10.2
Date: Fri, 17 Jul 2020 15:43:50 GMT
Last-Modified: Sun, 19 Apr 2020 08:52:37 GMT
Content-Type: image/jpeg
Content-Length: 101372
ETag: "5e9c1155-18bfc"
Accept-Ranges: bytes
```

##### 下载图片  
链接（网上找的，失效了就自己找一个）：http://pic.96weixin.com/ueditor/20200511/1589185236200665.jpg?x-oss-process=image/resize,m_lfit,w_120  

Response Headers

```
HTTP/1.1 200 OK
Server: TencentCOS
Date: Fri, 17 Jul 2020 15:42:07 GMT
Last-Modified: Mon, 11 May 2020 16:20:36 GMT
Content-Type: image/jpeg
Content-Length: 33302
Content-Disposition: attachment; filename*="UTF-8''1589185236200665.jpg"
```
这个响应头太多，我只截取关键部份。  

##### 对比不同
关键点在于下载图片多了

```
Content-Disposition: attachment; filename*="UTF-8''1589185236200665.jpg"
```
而1589185236200665.jpg就是我们下载图片保存到本地的文件名。

##### 结论
在常规的HTTP应答中，[Content-Disposition](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Disposition) 响应头指示回复的内容该以何种形式展示，是以内联的形式（即网页或者页面的一部分），还是以附件的形式下载并保存到本地。  
看了这么多，就是因为Content-Disposition指定了下载，所以就下载了，没有Content-Disposition响应头就是显示。
  
#### 阿里云OSS上传图片直接显示解决方案  

那么在阿里OSS上传又是怎么解决了，毕竟我们只上传了图片，也没有指定响应头。详细OSS的机制我已不想追究，没有意义，搞懂原理，只想睡个好觉，加班太JB累。  
网友给出的解决办法  

```
ObjectMetadata metadata = new ObjectMetadata();
// 指定Content-Type
metadata.setContentType(getContentType(path.substring(path.lastIndexOf("."))));
put.setMetadata(metadata);
```

```
    public static String getContentType(String FilenameExtension) {
        if (FilenameExtension.equalsIgnoreCase(".bmp")) {
            return "image/bmp";
        }
        if (FilenameExtension.equalsIgnoreCase(".gif")) {
            return "image/gif";
        }
        if (FilenameExtension.equalsIgnoreCase(".jpeg") ||
            FilenameExtension.equalsIgnoreCase(".jpg") ||
            FilenameExtension.equalsIgnoreCase(".png")) {
            return "image/jpg";
        }
        if (FilenameExtension.equalsIgnoreCase(".txt")) {
            return "text/plain";
        }
        return "image/jpg";
    }
```

-----


###### 转载请标明出处： 
###### http://blog.520wa.com/
###### 本文出自Tu's blog