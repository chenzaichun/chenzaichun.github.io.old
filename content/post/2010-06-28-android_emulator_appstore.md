+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-06-28T23:11:13+08:00"
status = "publish"
tags = ["android"]
title = "android模拟器中使用应用商店(appstore)"
type = "post"
description = ""
+++

如何在android模拟器中使用应用商店呢？

1. 去<a href="http://developer.htc.com/adp.html">http://developer.htc.com/adp.html</a>下载对应sdk的system.img，比如你装有1.6的android sdk，那么下载<a href="http://member.america.htc.com/download/RomCode/ADP/signed-dream_devphone_userdebug-img-14721.zip" target="_blank">http://member.america.htc.com/download/RomCode/ADP/signed-dream_devphone_userdebug-img-14721.zip?</a>

2. 将包里面的system.img解压缩到所建立的虚拟设备目录下，比如你的虚拟设备叫做htc，那么虚拟设备目录windows下在%APPLICATIONDATA%.androidavdhtc.avd下，linux在$HOME/.android/avd/htc.avd/下

3. 如果在创建虚拟设备的时候没有添加sdcard，不然会出现connection error错误！那么请添加sdcard，在虚拟设备目录下执行（前提当然是将android sdk中tools加入$PATH中）

```sh
mksdcard 512M sdcard.img
```

4. 启动模拟器，现在你就可以使用Google帐户登录appstore了，免费软件尽情下载安装吧！
<!--more-->