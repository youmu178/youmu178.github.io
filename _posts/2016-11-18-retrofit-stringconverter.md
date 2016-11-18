---
layout: post
category: android-development
title: Retrofit 添加返回数据为字符串的转换器
description: ""
modified: 2016-11-18
tags: [Retrofit]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit:
  creditlink:
comments: true
---

> **addConverterFactory(new ToStringConverterFactory())**

```java
public static class ToStringConverterFactory extends Converter.Factory {        
    static final MediaType MEDIA_TYPE = MediaType.parse("text/plain");       

    @Override        
    public Converter<ResponseBody, ?> responseBodyConverter(Type type, Annotation[] annotations, Retrofit retrofit) {
         if (String.class.equals(type)) {  
              return new Converter<ResponseBody, String>() {
                    @Override
                    public String convert(ResponseBody value) throws IOException {
                        return value.string();
                    } 
               };
            }
            return null;
        }

        @Override
        public Converter<?, RequestBody> requestBodyConverter(Type type, Annotation[] parameterAnnotations,
                                                              Annotation[] methodAnnotations, Retrofit retrofit) {
            if (String.class.equals(type)) {
                return new Converter<String, RequestBody>() {
                    @Override
                    public RequestBody convert(String value) throws IOException {
                        return RequestBody.create(MEDIA_TYPE, value); 
                   }
                };
            }
            return null;
        }
    }
```
