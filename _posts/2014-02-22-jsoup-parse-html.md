---
layout: post
category: android-development
title: 解析HTML
description: "解析网页HTML工具方法"
modified: 2014-02-22
tags: [HTML]
comments: true
share: true

comments: true
---
#一、作用

* (1) 从一个url、文件或string获得html并解析
* (2) 利用dom遍历或css选择器查找、提取数据
* (3) 操作html元素
* (4) 根据白名单去除用于提交的非法数据防止xss攻击
* (5) 输出整齐的html

#二、方法介绍

##1、解析一个HTML字符串

     Document doc = Jsoup.parse(html);
     提供了两种：
     (1) Jsoup.parse(String html)
     (2) Jsoup.parse(String html, String baseUri)
     第二种方法能够将输入的HTML解析为一个新的文档 (Document），参数 baseUri 是用来将相对 URL 转成绝对URL，并指定从哪个网站获取文档。如这个方法不适用，你可以使用 第一种方法来解析成HTML字符串。只要解析的不是空字符串，就能返回一个结构合理的文档，其中包含(至少) 一个head和一个body元素。
     一旦拥有了一个Document，你就可以使用Document中适当的方法或它父类 Element和Node中的方法来取得相关数据。
   

##2、解析一个body

     Document doc = Jsoup.parseBodyFragment(html);
     Element body = doc.body();
     parseBodyFragment 方法创建一个空壳的文档，并插入解析过的HTML到body元素中。假如你使用正常的 Jsoup.parse(String html) 方法，通常你也可以得到相同的结果，但是明确将用户输入作为 body片段处理，以确保用户所提供的任何糟糕的HTML都将被解析成body元素。
     Document.body() 方法能够取得文档body元素的所有子元素，与 doc.getElementsByTag("body")相同。
   
##3、从一个URL加载一个Document
     
	 Document doc = Jsoup.connect("http://example.com/").get();
     String title = doc.title();
     connect(String url) 方法创建一个新的 Connection, 和 get() 取得和解析一个HTML文件。如果从该URL获取HTML时发生错误，便会抛出 IOException，应适当处理。
     Connection 接口还提供一个方法链来解决特殊请求，具体如下：
     Document doc = Jsoup.connect("http://example.com")
                         .data("query", "Java")
                         .userAgent("Mozilla")
                         .cookie("auth", "token")
                         .timeout(3000)
                         .post();
     这个方法只支持Web URLs (http和https 协议); 假如你需要从一个文件加载，可以使用parse(File in, String charsetName) 代替。

##4、根据一个文件加载Document对象

     Jsoup.parse(File in, String charsetName, String baseUri) 
     File input = new File("/tmp/input.html");
     Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");
     
	 parse(File in, String charsetName, String baseUri) 这个方法用来加载和解析一个HTML文件。如在加载文件的时候发生错误，将抛出IOException，应作适当处理。
     baseUri 参数用于解决文件中URLs是相对路径的问题。如果不需要可以传入一个空的字符串。
     另外还有一个方法parse(File in, String charsetName) ，它使用文件的路径做为 baseUri。 这个方法适用于如果被解析文件位于网站的本地文件系统，且相关链接也指向该文件系统。

##5、使用dom方法来遍历一个Document对象

     File input = new File("/tmp/input.html");
     Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");

     Element content = doc.getElementById("content");
     Elements links = content.getElementsByTag("a");
     for (Element link : links) {
         String linkHref = link.attr("href");
         String linkText = link.text();
     }
###(1)查找元素

     getElementById(String id)
     getElementsByTag(String tag)
     getElementsByClass(String className)
     getElementsByAttribute(String key) (and related methods)
     Element siblings: siblingElements(), firstElementSibling(), lastElementSibling();nextElementSibling(), previousElementSibling()
     Graph: parent(), children(), child(int index)
	 
###(2)元素数据

     attr(String key)获取属性attr(String key, String value)设置属性
     attributes()获取所有属性
     id(), className() and classNames()
     text()获取文本内容text(String value) 设置文本内容
     html()获取元素内HTMLhtml(String value)设置元素内的HTML内容
     outerHtml()获取元素外HTML内容
     data()获取数据内容（例如：script和style标签)
     tag() and tagName()
	 
###(3)操作HTML和文本

     append(String html), prepend(String html)
     appendText(String text), prependText(String text)
     appendElement(String tagName), prependElement(String tagName)
     html(String value)

##6、使用选择器语法来查找元素
    
	 File input = new File("/tmp/input.html");
     Document doc = Jsoup.parse(input, "UTF-8", "http://example.com/");

     Elements links = doc.select("a[href]"); //带有href属性的a元素
     Elements pngs = doc.select("img[src$=.png]");//扩展名为.png的图片

     Element masthead = doc.select("div.masthead").first();//class等于masthead的div标签
     Elements resultLinks = doc.select("h3.r > a"); //在h3元素之后的a元素
     jsoup elements对象支持类似于CSS (或jquery)的选择器语法，来实现非常强大和灵活的查找功能。.
     这个select 方法在Document, Element,或Elements对象中都可以使用。且是上下文相关的，因此可实现指定元素的过滤，或者链式选择访问。
     Select方法将返回一个Elements集合，并提供一组方法来抽取和处理结果。
	 
###(1)Selector选择器概述

     tagname: 通过标签查找元素，比如：a
     ns|tag: 通过标签在命名空间查找元素，比如：可以用 fb|name 语法来查找 <fb:name> 元素
     #id: 通过ID查找元素，比如：#logo
     .class: 通过class名称查找元素，比如：.masthead
     [attribute]: 利用属性查找元素，比如：[href]
     [^attr]: 利用属性名前缀来查找元素，比如：可以用[^data-] 来查找带有HTML5 Dataset属性的元素
     [attr=value]: 利用属性值来查找元素，比如：[width=500]
     [attr^=value], [attr$=value], [attr*=value]: 利用匹配属性值开头、结尾或包含属性值来查找元素，比如：[href*=/path/]
     [attr~=regex]: 利用属性值匹配正则表达式来查找元素，比如： img[src~=(?i)\.(png|jpe?g)]
     *: 这个符号将匹配所有元素

###(2)Selector选择器组合使用

     el#id: 元素+ID，比如： div#logo
     el.class: 元素+class，比如： div.masthead
     el[attr]: 元素+class，比如： a[href]
	 
###(3)任意组合，比如：a[href].highlight
     ancestor child: 查找某个元素下子元素，比如：可以用.body p 查找在"body"元素下的所有p元素
     parent > child: 查找某个父元素下的直接子元素，比如：可以用div.content > p 查找 p 元素，也可以用body > * 查找body标签下所有直接子元素
     siblingA + siblingB: 查找在A元素之前第一个同级元素B，比如：div.head + div
     siblingA ~ siblingX: 查找A元素之前的同级X元素，比如：h1 ~ p
     el, el, el:多个选择器组合，查找匹配任一选择器的唯一元素，例如：div.masthead, div.logo
	 
###(4)伪选择器selectors

     :lt(n): 查找哪些元素的同级索引值（它的位置在DOM树中是相对于它的父节点）小于n，比如：td:lt(3) 表示小于三列的元素
     :gt(n):查找哪些元素的同级索引值大于n，比如： div p:gt(2)表示哪些div中有包含2个以上的p元素
     :eq(n): 查找哪些元素的同级索引值与n相等，比如：form input:eq(1)表示包含一个input标签的Form元素
     :has(seletor): 查找匹配选择器包含元素的元素，比如：div:has(p)表示哪些div包含了p元素
     :not(selector): 查找与选择器不匹配的元素，比如： div:not(.logo) 表示不包含 class=logo 元素的所有 div 列表
     :contains(text): 查找包含给定文本的元素，搜索不区分大不写，比如： p:contains(jsoup)
     :containsOwn(text): 查找直接包含给定文本的元素
     :matches(regex): 查找哪些元素的文本匹配指定的正则表达式，比如：div:matches((?i)login)
     :matchesOwn(regex): 查找自身包含文本匹配指定正则表达式的元素
     注意：上述伪选择器索引是从0开始的，也就是说第一个元素索引值为0，第二个元素index为1等
	 
##7、从元素抽取属性，文本和HTML

     *要取得一个属性的值，可以使用Node.attr(String key) 方法
     *对于一个元素中的文本，可以使用Element.text()方法
     *对于要取得元素或属性中的HTML内容，可以使用Element.html(), 或 Node.outerHtml()方法
	 
     示例：
      String html = "<p>An <a href='http://example.com/'><b>example</b></a> link.</p>";
      Document doc = Jsoup.parse(html);//解析HTML字符串返回一个Document实现
      Element link = doc.select("a").first();//查找第一个a元素

      String text = doc.body().text(); // "An example link"//取得字符串中的文本
      String linkHref = link.attr("href"); // "http://example.com/"//取得链接地址
      String linkText = link.text(); // "example""//取得链接地址中的文本

      String linkOuterH = link.outerHtml(); // "<a href="http://example.com"><b>example</b></a>"
      String linkInnerH = link.html(); // "<b>example</b>"//取得链接内的html内容
     上述方法是元素数据访问的核心办法。此外还其它一些方法可以使用：
      Element.id()
      Element.tagName()
      Element.className() and Element.hasClass(String className)
     这些访问器方法都有相应的setter方法来更改数据.
	 
##8、URL 处理

     *在你解析文档时确保有指定base URI，然后
     *使用 abs: 属性前缀来取得包含base URI的绝对路径。代码如下： 
     Document doc = Jsoup.connect("http://www.open-open.com").get();

     Element link = doc.select("a").first();
     String relHref = link.attr("href"); // == "/"
     String absHref = link.attr("abs:href"); // "http://www.open-open.com/"
     在HTML元素中，URLs经常写成相对于文档位置的相对路径： <a href="/download">...</a>. 当你使用 Node.attr(String key) 方法来取得a元素的href属性时，它将直接返回在HTML源码中指定定的值。
     假如你需要取得一个绝对路径，需要在属性名前加 abs: 前缀。这样就可以返回包含根路径的URL地址attr("abs:href")
     因此，在解析HTML文档时，定义base URI非常重要。
     如果你不想使用abs: 前缀，还有一个方法能够实现同样的功能 Node.absUrl(String key)
	 
# 更多请参考官网 
　　 [API](http://jsoup.org/apidocs/)<br/>
           [jar包](http://jsoup.org/download)
