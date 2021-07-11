+++
categories = ["programming"]
comments = true
published = true
status = "publish"
tags = ["android", "Google"]
title = "Android地图MapView"
date = "2010-10-10T17:17:17+08:00"
type = "post"
description = ""
+++

今天想在android使用Google地图，查了一下，难点就是要自己去申请一个api key，不然是不能够正确获取地图服务的。方法如下：

1. 查看debug key的md5 finger，如果在windows下，自己去“打开Eclipse--->Windows--->Preferences--->Android--->Build，查看默认的debug keystore位置”

```sh
keytool -list -alias androiddebugkey -keystore ~/.android/debug.keystore" -storepass android -keypass android
```

得到
<!--more-->

``` 
androiddebugkey, 2010-9-14, PrivateKeyEntry,
认证指纹 (MD5)： 67:09:76:74:C5:45:C2:F1:CC:93:15:AA:1A:CA:66:8F
```

2. 到<a href="http://code.google.com/android/maps-api-signup.html">http://code.google.com/android/maps-api-signup.html</a>申请api key，输入上面得到的md5 finger

3. 在layout中加入MapView

```xml
<com.google.android.maps.MapView
   		 android:id="@+id/map"
                 android:layout_width="fill_parent"
                 android:layout_height="fill_parent"
                 android:apiKey="0ChzlZ4HDjbb9arHGkqJ2gzUcmn56yoRKJAlZNA"
                 android:clickable="true" />
                 />
```

4. 修改AndroidMainifest.xml，在<span style="color: #ff0000;">appliction标签</span>之间加入

```xml
<uses-library android:name="com.google.android.maps" /> 
```

5. 创建一个activity派生自MapActivity，根据eclipse的提示默认重载两个函数就行了

6. build，然后运行程序，是不是就可以看见地图了！

代码下载: <a href="http://commondatastorage.googleapis.com/czc_public/code/android/maptest.tar.gz" target="_blank">猛击这里</a>
