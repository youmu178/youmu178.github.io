---
layout: post
category: android-development
title: How to create simple view separators
description: ""
modified: 2014-06-24
tags: [android]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit: 
  creditlink: 
comments: true
---


##介绍

这将向您展示两种方法可以创建漂亮的分隔符,例如一排按钮之间使用。

(https://raw.githubusercontent.com/ITBox/Picture/master/buttons-with-separators3.png)

##方法1  手动添加一个视图分离器LinearLayout
作为一个分隔符,我们可以使用一个视图如下:

{% highlight xml %}

<View
   android:layout_height="fill_parent"
   android:layout_width="1dp"
   android:background="#90909090"
   android:layout_marginBottom="5dp"
   android:layout_marginTop="5dp"
/>

{% endhighlight %}

整个布局,如图所示,就变成:

{% highlight xml %}

 <LinearLayout
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:adjustViewBounds="true"
    android:orientation="horizontal">
 
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        style="?android:attr/buttonBarButtonStyle"
        android:text="Yes"
        android:layout_weight="1"
        android:id="@+id/button1"
        android:textColor="#00b0e4" />
 
    <View android:layout_height="fill_parent"
        android:layout_width="1px"
        android:background="#90909090"
        android:layout_marginBottom="5dp"
        android:layout_marginTop="5dp"
        android:id="@+id/separator1" />
 
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        style="?android:attr/buttonBarButtonStyle"
        android:text="No"
        android:layout_weight="1"
        android:id="@+id/button2"
        android:textColor="#00b0e4" />
 
    <View android:layout_height="fill_parent"
        android:layout_width="1px"
        android:background="#90909090"
        android:layout_marginBottom="5dp"
        android:layout_marginTop="5dp"
        android:id="@+id/separator2" />
 
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        style="?android:attr/buttonBarButtonStyle"
        android:text="Neutral"
        android:layout_weight="1"
        android:id="@+id/button3"
        android:textColor="#00b0e4" />
 
 </LinearLayout>

{% endhighlight %}

##方法2  指定一个分压器LinearLayout

您可以指定一个视图linearlayout分配器。这显然是一个更好的解决方案(和我寻找的解决方案),还可以锻炼当你处理一个未知数量的Items。
这种方法只适用于API 11和更高的水平。
* 创建一个叫separator.xml的drawable文件:
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
   <size android:width="1dp" />
   <solid android:color="#90909090" />
</shape>
{% endhighlight %}

* 我们可以这样在布局中用

{% highlight xml %}
<LinearLayout
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:adjustViewBounds="true"
    android:divider="@drawable/separator"
    android:showDividers="middle"
    android:orientation="horizontal">
 
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        style="?android:attr/buttonBarButtonStyle"
        android:text="Yes"
        android:layout_weight="1"
        android:id="@+id/button1"
        android:textColor="#00b0e4" />
 
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        style="?android:attr/buttonBarButtonStyle"
        android:text="No"
        android:layout_weight="1"
        android:id="@+id/button2"
        android:textColor="#00b0e4" />
 
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        style="?android:attr/buttonBarButtonStyle"
        android:text="Neutral"
        android:layout_weight="1"
        android:id="@+id/button3"
        android:textColor="#00b0e4" />
 
</LinearLayout>
{% endhighlight %}

##这里的重要部分是:
{% highlight xml %}
      android:divider="@drawable/separator"
      android:showDividers="middle"
{% endhighlight %}
