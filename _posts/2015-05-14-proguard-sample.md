---
layout: post
category: android-development
title: 各框架库混淆
description: ""
modified: 2015-05-14
tags: [proguard]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit:
  creditlink:
comments: true
---

### 用到了一库，混淆的时候编译会出问题，特此写下我用到过的混淆代码
- Fragement
```
#如果引用了v4或者v7包
# Keep the support library
-keep class android.support.v4.** { *; }
-keep interface android.support.v4.** { *; }
# Keep the support library
-keep class android.support.v7.** { *; }
-keep interface android.support.v7.** { *; }
```
- ButterKnife 6.0
```
    -keep class butterknife.** { *; }
    -dontwarn butterknife.internal.**
    -keep class **$$ViewInjector { *; }
    -keepclasseswithmembernames class * {
            @butterknife.* <fields>;
    }
    -keepclasseswithmembernames class * {
       @butterknife.* <methods>;
    }
```
-ButterKnife 7.0
```
-keep class butterknife.** { *; }
-dontwarn butterknife.internal.**
-keep class **$$ViewBinder { *; }
-keepclasseswithmembernames class * {  
   @butterknife.* <fields>;
}
-keepclasseswithmembernames class * {   
 @butterknife.* <methods>;
}
```
- ActiveAndroid
```   
 -libraryjars libs/activeandroid.jar
 -dontwarn com.activeandroid.**
 -keep class com.activeandroid.** {*;}
```
- EventBus
```
   -keepclassmembers class ** {
       public void onEvent*(**);
    }
    # Only required if you use AsyncExecutor
    -keepclassmembers class * extends de.greenrobot.event.util.ThrowableFailureEvent {
       <init>(java.lang.Throwable);
    }
```
- Gson
```
    ## GSON 2.2.4 specific rules ##
    # Gson uses generic type information stored in a class file when working with fields. Proguard
    # removes such information by default, so configure it to keep all of it.
    -keepattributes Signature
    # For using GSON @Expose annotation
    -keepattributes *Annotation*
    # Gson specific classes
    -keep class sun.misc.Unsafe { *; }
    -keep class com.google.gson.stream.** { *; }
    # Application classes that will be serialized/deserialized over Gson
    -keep class com.sunloto.shandong.bean.** { *; }
```
- Retrofit
```
    -dontwarn retrofit.**
    -keep class retrofit.** { *; }
    -keepattributes Signature
    -keepattributes Exceptions
```
- Picasso
```
  -dontwarn com.squareup.okhttp.**
```
- pingyin4j
```
    -dontwarn net.soureceforge.pinyin4j.**
    -dontwarn demo.**
    -libraryjars libs/pinyin4j-2.5.0.jar
    -keep class net.sourceforge.pinyin4j.** { *;}
    -keep class demo.** { *;}
```
- 华为push
```
    -libraryjars libs/CloudPush_SDK_V2509_hi.jar
    -dontwarn com.huawei.**
    -keep class com.huawei.** {*;}
```
- 友盟统计
```
   -keepclassmembers class * {
        public <init>(org.json.JSONObject);
    }
    -keep public class [您的应用包名].R$*{
        public static final int *;
    }
    -keepclassmembers enum * {
        public static **[] values();
        public static ** valueOf(java.lang.String);
    }
```
- 友盟自动更新
```
    -keep public class [您的应用包名].R$*{
       public static final int *;
    }
    -keep public class com.umeng.fb.ui.ThreadView {
    }
    如果仍存在问题，可以再做如下操作：
    # 添加第三方jar包
    -libraryjars libs/umeng-sdk.jar
    # 以下类过滤不混淆  
    -keep public class * extends com.umeng.**
    # 以下包不进行过滤
    -keep class com.umeng.** { *; }
```
- 友盟用户反馈
```
    -keepclassmembers class * {
        public <init>(org.json.JSONObject);
    }
    #把[您的应用包名] 替换成您自己的包名，如"com.example.R$*"。
    -keep public class 您的应用包名.R$*{
       public static final int *;
    }
```
- 友盟消息推送
```
    -dontshrink
    -keep,allowshrinking class com.umeng.message.* {
        public <fields>;
        public <methods>;
    }
    -keep,allowshrinking class com.umeng.message.protobuffer.MessageResponse$PushResponse$Info {
        public <fields>;
        public <methods>;
    }
    -keep,allowshrinking class com.umeng.message.protobuffer.MessageResponse$PushResponse$Info$Builder {
        public <fields>;
        public <methods>;
    }
    -keep,allowshrinking class org.android.agoo.impl.*{
        public <fields>;
        public <methods>;
    }
    -keep,allowshrinking class org.android.agoo.service.* {*;}
    -keep,allowshrinking class org.android.spdy.**{*;}
    -keep public class [应用包名].R$*{
        public static final int *;
    }
```
- 极光推送
```
-dontwarn cn.jpush.**
-keep class cn.jpush.** { *; }
```
- 其他
```
# 源文件和行号的信息不混淆
-keepattributes SourceFile,LineNumberTable
```
