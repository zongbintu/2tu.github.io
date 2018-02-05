title: DataBinding setDrawable 无法显示的问题
date: 2015-10-14 09:44:13  
categories: Android  
tags: [DataBinding]
---


* 问题：dataBinding中drawableLeft无效

* 解决办法：采用传统代码设置

* 思路：

1、使用代码设置Drawable有两种方式。
drawable.setBounds(int,int,int,int);//未设置则不显示
view.setCompoundDrawables(drawable,null,null,null);

view.setCompoundDrawablesWithIntrinsicBounds(drawable,null,null,null);
<!--more-->  
2、查看生成的binding代码发现采用setCompoundDrawables方式，且未设置Bounds


* 使用到的代码<br/>
android:drawableRight="@{1==employee.state ?@drawable/ic_job : @drawable/ic_job_quit}"

TextViewBindingAdapter

    @BindingAdapter({"android:drawableRight"})
    public static void setDrawableRight(TextView view, Drawable drawable) {
        Drawable[] drawables = view.getCompoundDrawables();
        view.setCompoundDrawables(drawables[0], drawables[1], drawable, drawables[3]);
    }
    
    
    
* 资料<br/>
https://code.google.com/p/android-developer-preview/issues/detail?id=3175&can=1&q=status%3Afixed%20and%20opened-after%3A2015%2F4%2F1&sort=-opened&colspec=ID%20Type%20Status%20Owner%20Summary%20Opened*