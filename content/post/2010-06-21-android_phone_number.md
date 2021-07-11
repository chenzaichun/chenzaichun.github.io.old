+++
categories = ["programming", "tools"]
comments = true
date = "2010-06-21T13:21:33+08:00"
published = true
status = "publish"
tags = ["android"]
title = "android获取手机号码"
type = "post"
description = ""
+++


android下获取手机的号码

```java
//创建电话管理
TelephonyManager tm = (TelephonyManager);
//与手机建立连接
activity.getSystemService(Context.TELEPHONY_SERVICE);

//获取手机号码
String phoneId = tm.getLine1Number();
```

在manifest file中添加

```xml
<!--  记得在manifest file中添加 -->
<uses-permission   android:name="android.permission.READ_PHONE_STATE" />
```

程序在模拟器上无法获取，必须连接手机

参考连接：

<a href="http://hi.baidu.com/hencechen/blog/item/e4d983094c01b4c73ac76349.html" target="_blank">http://hi.baidu.com/hencechen/blog/item/e4d983094c01b4c73ac76349.html</a>
<!--more-->