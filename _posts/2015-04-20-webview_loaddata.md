---
layout: post
category: android-development
title: 动态更改Html文本用WebView加载
description: ""
modified: 2015-04-20
tags: [WebView]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit:
  creditlink:
comments: true
---

- 在HTML中，需要更改的位置用占位符表示（名字随便起）
- 把HTML转化成字符串，动态替换占位符
- 用loadDataWithBaseURL加载

----------

```
public static String htmlConvert(Context ctx) {
		StringBuilder sb = null;
		BufferedReader br = null;
		try {
			br = new BufferedReader(new InputStreamReader(ctx.getAssets().open("play/view/information.html")));
			sb = new StringBuilder();
			String line = null;
			while ((line = br.readLine()) != null) {
				sb.append(line);
			}
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			try {
				br.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		return sb.toString();
	}
  ```

----------


- 用WebView加载

```
webView.loadDataWithBaseURL(null, html, "text/html", "utf-8", null);`

```
