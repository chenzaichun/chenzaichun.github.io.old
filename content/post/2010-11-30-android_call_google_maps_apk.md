+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-11-30T12:37:11+08:00"
tags = ["android", "Google"]
title = "android下程序直接调用Google自己的地图程序"
type = "post"
description = ""
+++

直接贴代码吧

```java
Intent intent = new Intent(Intent.ACTION_MAIN);
ComponentName com = new ComponentName(
        		"com.google.android.apps.maps", //要打开的程序的包名
        		"com.google.android.maps.MapsActivity" //程序中入口MainActivity，在Mainifent.xml查看
        		);
intent.setComponent(com);
startActivity(intent);
```
<!--more-->