+++
categories = ["gamedev", "programming"]
comments = true
published = true
status = "publish"
date = "2010-06-29T12:21:11+08:00"
tags = ["android"]
title = "android屏幕旋转问题"
type = "post"
description = ""
+++

转自：<a href="http://blog.csdn.net/jump_1990/archive/2009/11/04/4766853.aspx" target="_blank">http://blog.csdn.net/jump_1990/archive/2009/11/04/4766853.aspx</a>

要让程序界面保持一个方向，不随手机方向转动而变化的处理办法：<br>在AndroidManifest.xml里面配置一下就可以了。加入这一行android:screenOrientation="landscape"。

例如（landscape是横向，portrait是纵向）：

```xml
<?xml version="1.0" encoding="utf-8"?> 
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
      package="com.ray.linkit" 
      android:versionCode="1" 
      android:versionName="1.0"> 
    <application android:icon="@drawable/icon" android:label="@string/app_name"> 
        <activity android:name=".Main" 
                  android:label="@string/app_name" 
                  android:screenOrientation="portrait"> 
            <intent-filter> 
                <action android:name="android.intent.action.MAIN" /> 
                <category android:name="android.intent.category.LAUNCHER" /> 
            </intent-filter> 
        </activity> 
                <activity android:name=".GamePlay" 
                android:screenOrientation="portrait"></activity> 
                <activity android:name=".OptionView" 
                android:screenOrientation="portrait"></activity> 
    </application> 
    <uses-sdk android:minSdkVersion="3" /> 
</manifest>
```

另外，android中每次屏幕的切换都会重启Activity，所以应该在Activity销毁前保存当前活动的状态，在Activity再次 Create的时候载入配置，那样，进行中的游戏就不会自动重启了！也可以给每个activity加上 android:configChanges="keyboardHidden|orientation"属性，就不会重启activity，而是去调用 onConfigurationChanged(Configuration newConfig)。这样就可以在这个方法里调整显示方式：

```java
if(newConfig.orientation==Configuration.  
```
<!--more-->