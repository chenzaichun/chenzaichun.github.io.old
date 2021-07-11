+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-12T18:19:23+08:00"
status = "publish"
tags = ["python"]
title = "windows下通过python获取用户桌面路径"
type = "post"
description = ""
+++


```python
from win32com.shell import shell

def GetDesktoppath():
    ilist = shell.SHGetSpecialFolderLocation(0,shellcon.CSIDL_DESKTOP)
    dtpath = shell.SHGetPathFromIDList(ilist)
    return dtpath
```
<!--more-->