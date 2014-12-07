---
layout: post
category: android-development
title: ThinDownloadManager
description: ""
modified: 2014-12-07
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

主要是下载文件用的
[https://github.com/smanikandan14/ThinDownloadManager][项目地址]

##用法
###**DownloadStatusListener**

*监听完成，失败，进度 

``` java

  void onDownloadComplete (int id);
  
  void onDownloadFailed (int id, int errorCode, String errorMessage);
  
  void onProgress (int id, long totalBytes, long downlaodedBytes, int progress); 

```

###**DownloadRequest**

  * 设置下载请求
  * 下载地址 URI, 目的地 URI.
  * 设置请求优先级 HIGH or MEDIUM or LOW.
  * 设置回调监听器 DownloadStatusListener


     ``` java
        Uri downloadUri = Uri.parse("http://tcrn.ch/Yu1Ooo1");
        Uri destinationUri = Uri.parse(this.getExternalCacheDir().toString()+"/test.mp4");
        DownloadRequest downloadRequest = new DownloadRequest(downloadUri)
                .setDestinationURI(destinationUri).setPriority(DownloadRequest.Priority.HIGH)
                .setDownloadListener(new DownloadStatusListener() {
                    @Override
                    public void onDownloadComplete(int id) {
                        
                    }

                    @Override
                    public void onDownloadFailed(int id, int errorCode, String errorMessage) {
                        
                    }

                    @Override
                    public void onProgress(int id, long totalBytes, long downlaodedBytes, int progress)) {
                        
                    }
                });

     ```
	 
###**ThinDownloadManager** 

  * 可以设置线程池大小  1 - 4. 默认是 1.
  ``` java
    private ThinDownloadManager downloadManager;
    private static final int DOWNLOAD_THREAD_POOL_SIZE = 2;
    
    .....
    
    downloadManager = new ThinDownloadManager(DOWNLOAD_THREAD_POOL_SIZE);
    
    ....
```

  * 开始添加请求 *add( DownloadRequest request)*
   	```java
   	int downloadId = downloadManager.add(downloadRequest);
   	```

  * 根据ID结束下载 *cancel(int downloadId)* by passing download id. 
  	- Returns 1 if successfull cancelled.
  	- Returns -1 if supplied download id is not found.
  	
  	```java
  	int status = downloadManager.cancel(downloadId);
  	```

  * 结束所有的下载请求 use *cancelAll()*
  	```java
  	downloadManager.cancelAll();
  	```

  * 查询下载状态 *query(int downloadId)*
  
    The possible status could be
  	- STATUS_PENDING
  	- STATUS_STARTED
  	- STATUS_RUNNING
  	
  	```java
  	int status = downloadManager.query(downloadId);
  	```
  * 释放资源 *release()*.
  	
  	```java
  	downloadManager.release();
  	```