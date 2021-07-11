+++
categories = ["programming"]
comments = true
published = true
status = "publish"
date = "2010-06-27T23:22:11+08:00"
tags = ["android"]
title = "android设置APN"
type = "post"
description = ""
+++


在android下如何通过程序设置APN连接呢，比如我想通过一个私有的APN连接到网络。查询到的结果是：

1. 第一步，创建activity，使用如下代码设置APN： 


```java
ContentValues values = new ContentValues();
values.put("NAME", "CMCC");
values.put("APN", "CMCC");
values.put("PROXY", "192.168.0.171");
values.put("PORT", "80");
values.put("USER", "");
values.put("PASSWORD", "");
this.getContentResolver().insert(Uri.parse("content://telephony/carriers"), values);
```

2. 在AndroidManifest.xml中添加如下内容：

```xml
<uses-permission android:name="android.permission.WRITE_APN_SETTINGS" />
```
<!--more-->