+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-07-22T16:23:11+08:00"
tags = ["VC", "Windows"]
title = "windows获取任务栏高度"
type = "post"
description = ""
+++


```cpp
RECT Rect;
HWND hWnd = FindWindow("Shell_TrayWnd", 0);
if (GetWindowRect(hWnd, &Rect)) {
        //Rect.bottom-Rect.top   就是任务栏的高度
}
```
<!--more-->