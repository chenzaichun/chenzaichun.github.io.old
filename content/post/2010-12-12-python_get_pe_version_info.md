+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-12T19:22:11+08:00"
status = "publish"
tags = ["python"]
title = "python获取PE文件版本信息"
type = "post"
description = ""
+++


通过pywin32中的win32api.GetFileVersionInfo来获取

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# Get the version information from Windows PE

from win32api import GetFileVersionInfo, LOWORD, HIWORD

def get_version_number (filename):
    try:
        info = GetFileVersionInfo (filename, "\")
        ms = info['FileVersionMS']
        ls = info['FileVersionLS']
        return HIWORD (ms), LOWORD (ms), HIWORD (ls), LOWORD (ls)
    except:
        return 0,0,0,0

def printOtherInfo(filename):
    try:
        info = GetFileVersionInfo(filename, "\")
        print(info)

	# In order to get the company information, we need to get the lang 
	# and code page first, then get the related strings
        trans = GetFileVersionInfo(filename, "\VarFileInfo\Translation")
        if not trans:
            return 
        print(info)

        # Common string lists:
        # "CompanyName","FileDescription", "FileVersion", "InternalName",
        # "LegalCopyright", "OriginalFilename", "ProductName", "ProductVersion"
        # You can use this way to get custom defined strings.

        info = GetFileVersionInfo(filename, "\StringFileInfo\%04x%04x\%s" % (trans[0][0], trans[0][1], "CompanyName"))
        print(info)
        info = GetFileVersionInfo(filename, "\StringFileInfo\%04x%04x\%s" % (trans[0][0], trans[0][1], "FileVersion"))
        print(info)
    except:
        pass

if __name__ == '__main__':
    import os
    filename = os.environ["COMSPEC"]
    print ".".join ([str (i) for i in get_version_number (filename)])
    printOtherInfo(filename)
```
<!--more-->