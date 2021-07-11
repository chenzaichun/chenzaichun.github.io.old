+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-07-22T23:11:37+08:00"
tags = ["VC", "Windows"]
title = "在任务栏中隐藏windows程序"
type = "post"
description = ""
+++

直接代码：

```cpp
ModifyStyleEx(WS_EX_APPWINDOW,WS_EX_TOOLWINDOW, SWP_DRAWFRAME);
```
<!--more-->