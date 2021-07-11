+++
categories = ["gamedev", "Linux", "programming", "tools"]
comments = true
published = true
date = "2010-07-21T17:23:37+08:00"
status = "publish"
tags = ["android"]
title = "解决Error executing aapt. Please check aapt is present at %错误"
type = "post"
description = ""
+++


原文：<a href="http://blog.csdn.net/ovsynnia/archive/2007/12/06/1920365.aspx" target="_blank">猛击这里</a>

根据指南先配置好JDK，和设置path，并且更新sdk要重新更改path，按照指南来还出现问题！

Error executing aapt. Please check aapt is present at %

解决方案：在eclipse–>window–>属性–>android–>设置sdk的路径为解压缩的`android_sdk_<platform>_<release>_<build>`.确定ＯＫ　小红叉没有了

但是在linux下还是出现了这个问题（因为更新了sdk），后来发现是因为platform/android-1.x/tools/下的工具没有执行权限

```sh
chmod +x *
```
<!--more-->