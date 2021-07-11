+++
categories = ["programming"]
comments = true
published = true
date = "2010-11-30T17:27:32+08:00"
status = "publish"
tags = ["android", "Google"]
title = "android调用Google Maps导航"
type = "post"
description = ""
+++

直接代码：

```java
String s = String.format("http://maps.google.com/maps?f=d&saddr=jiangbei,chongqing&daddr=shapingba,chongqing&hl=en");
Uri uri = Uri.parse(s);
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);
```
<!--more-->