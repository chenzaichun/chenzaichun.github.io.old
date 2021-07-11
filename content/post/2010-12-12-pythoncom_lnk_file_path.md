+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-12T19:32:11+08:00"
status = "publish"
tags = ["python"]
title = "在windows下通过pythoncom获取快捷方式所指定的路径"
type = "post"
description = ""
+++

直接代码：

```python
import pythoncom

def GetURLFromShortcut(url):
    shortcut = pythoncom.CoCreateInstance(
                                shell.CLSID_ShellLink, None,
        			pythoncom.CLSCTX_INPROC_SERVER, shell.IID_IShellLink)
    shortcut.QueryInterface( pythoncom.IID_IPersistFile ).Load(url)
    url = shortcut.GetPath(shell.SLGP_SHORTPATH)[0]

    return url
```
<!--more-->