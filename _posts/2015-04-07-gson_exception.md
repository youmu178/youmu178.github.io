---
layout: post
category: android-development
title: 使用GSON转List
description: ""
modified: 2015-04-07
tags: [GSON]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit: 
  creditlink: 
comments: true
---

 * 在使用GSON转List的时候，遇到一个问题，传入泛型转List，GSON是无法解析的，GSON不知道你传的是什么，将会按原样返给你。 
 
 eg: 
 
 {% highlight java %} 
 
    public static <T> List<T> getList(String json, Class<T> clasz) {
		 return mGson.fromJson(json, new TypeToken<List<T>>() {}.getType());
	} 
	
 {% endhighlight %}
 
 * 经鲍永章同学Help me，得以解决， 
 
 [stackoverflow](http://stackoverflow.com/questions/14139437/java-type-generic-as-argument-for-gson) 
 
 * 具体方法如下： 
 
  {% highlight java %} 
  
    public static <T> List<T> getList(String json, Class<T> clasz) {
		return mGson.fromJson(json, new ListOfSomething<T>(clasz));
	} 
	
	static class ListOfSomething<T> implements ParameterizedType {

		private Class<?> wrapped;

		public ListOfSomething(Class<T> wrapped) {
			this.wrapped = wrapped;
		}

		public Type[] getActualTypeArguments() {
			return new Type[] { wrapped };
		}

		public Type getRawType() {
			return List.class;
		}

		public Type getOwnerType() {
			return null;
		}
	}
   {% endhighlight %}
 
