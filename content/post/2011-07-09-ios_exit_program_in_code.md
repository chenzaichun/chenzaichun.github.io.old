+++
categories = ["ios", "programming"]
comments = true
published = true
status = "publish"
date = "2011-07-09T20:56:31+08:00"
tags = ["ios"]
title = "iOS退出程序"
type = "post"
description = ""
+++

如果想在程序里面添加一个按钮“退出”，可以调用

```objc
[[UIApplication sharedApplication] terminateWithSuccess];
```

在cocos2d-iphone中添加退出按钮后的响应函数：

```objc
[[CCDirector sharedDirector] end];
[[UIApplication sharedApplication] terminateWithSuccess];
```
<!--more-->