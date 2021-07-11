+++
categories = ["programming", "tools"]
comments = true
date = "2010-07-22T18:33:16+08:00"
published = true
status = "publish"
tags = ["VC", "Windows"]
title = "windows下获取屏幕分辨率"
type = "post"
description = ""
+++


windows下获取屏幕分辨率:

```cpp
int nWidth = GetSystemMetrics(SM_CXSCREEN); //屏幕宽度   
int nHeight = GetSystemMetrics(SM_CYSCREEN); //屏幕高度
```
<!--more-->