---
layout: post
category: android-development
title: Irregular Shapes（不规则形状图片）
description: ""
modified: 2014-07-21
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

这将向您展示2种形状。一种圆角图片，一种貌似聊天气泡的图片，带把的。

(https://raw.githubusercontent.com/ITBox/Picture/master/Irregular%20Shapes.jpg)
(https://raw.githubusercontent.com/ITBox/Picture/master/Irregular%20Shapes2.jpg)

#首先我们要了解位图的渲染器（BitmapShader）
*public   BitmapShader(Bitmap bitmap,Shader.TileMode tileX,Shader.TileMode tileY)

*bitmap   在渲染器内使用的位图

*tileX      The tiling mode for x to draw the bitmap in.   在位图上X方向花砖模式

*tileY     The tiling mode for y to draw the bitmap in.    在位图上Y方向花砖模式

*TileMode：（一共有三种）

*CLAMP  ：如果渲染器超出原始边界范围，会复制范围内边缘染色。

*REPEAT ：横向和纵向的重复渲染器图片，平铺。

*MIRROR ：横向和纵向的重复渲染器图片，这个和REPEAT 重复方式不一样，他是以镜像方式平铺。

##第一种

{% highlight java %}

public Bitmap processImage(Bitmap bitmap) {
    Bitmap bmp;
 
    bmp = Bitmap.createBitmap(bitmap.getWidth(), 
        bitmap.getHeight(), Bitmap.Config.ARGB_8888);
    BitmapShader shader = new BitmapShader(bitmap, 
        BitmapShader.TileMode.CLAMP, 
        BitmapShader.TileMode.CLAMP);
 
    float radius = Math.min(bitmap.getWidth(), 
        bitmap.getHeight()) / RADIUS_FACTOR;
    Canvas canvas = new Canvas(bmp);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setShader(shader);
 
    RectF rect = new RectF(0, 0, 
        bitmap.getWidth(), bitmap.getHeight());
    canvas.drawRoundRect(rect, radius, radius, paint);
 
    return bmp;
}

{% endhighlight %}

##第二种 

主要是画那个带把的三角形
* 画三角形我们首先创建一个空的路径:
Path triangle = new Path();

* 接下来,我们使用moveto()当前指向的起始位置。在这种情况下我们要搬到三角形的点左边缘的画布,一点点从顶部画。这是最外层的讲话泡沫三角形:
triangle.moveTo(0, TRIANGLE_OFFSET);

* 现在我们使用lineTo()从当前位置画一条线(我们在themoveTo())的右边缘边缘区域,向上倾斜:
triangle.lineTo(TRIANGLE_WIDTH, TRIANGLE_OFFSET - (TRIANGLE_HEIGHT / 2));

* 现在我们使用第二个画线()从当前位置画一个二线(thelineTo()划了一道线,但不改变当前位置)。这一次我们希望线斜率向下,镜像的第一行:
triangle.lineTo(TRIANGLE_WIDTH, TRIANGLE_OFFSET + (TRIANGLE_HEIGHT / 2));

* 最后,我们想要画最后一行的最后边三角形。最明显的方法是使用一个函数()转移到一个端点的前两行,然后做画线画一条线的端点其他线。然而,有一个更简单的方法。如果我们调用close()的道路上它会自动关闭划一条线两个开放结束当前的路径,这将画的线,我们所需要的
triangle.close();

{% highlight java %}
private static final float RADIUS_FACTOR = 8.0f;
private static final int TRIANGLE_WIDTH = 120;
private static final int TRIANGLE_HEIGHT = 100;
private static final int TRIANGLE_OFFSET = 300;

public Bitmap processImage(Bitmap bitmap) {
    Bitmap bmp;

    bmp = Bitmap.createBitmap(bitmap.getWidth(), 
        bitmap.getHeight(), Bitmap.Config.ARGB_8888);
    BitmapShader shader = new BitmapShader(bitmap, 
        BitmapShader.TileMode.CLAMP, 
        BitmapShader.TileMode.CLAMP);

    float radius = Math.min(bitmap.getWidth(), 
        bitmap.getHeight()) / RADIUS_FACTOR;
    Canvas canvas = new Canvas(bmp);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setShader(shader);

    RectF rect = new RectF(TRIANGLE_WIDTH, 0, 
        bitmap.getWidth(), bitmap.getHeight());
    canvas.drawRoundRect(rect, radius, radius, paint);

    Path triangle = new Path();
    triangle.moveTo(0, TRIANGLE_OFFSET);
    triangle.lineTo(TRIANGLE_WIDTH, 
        TRIANGLE_OFFSET - (TRIANGLE_HEIGHT / 2));
    triangle.lineTo(TRIANGLE_WIDTH, 
        TRIANGLE_OFFSET + (TRIANGLE_HEIGHT / 2));
    triangle.close();
    canvas.drawPath(triangle, paint);

    return bmp;
}
{% endhighlight %}