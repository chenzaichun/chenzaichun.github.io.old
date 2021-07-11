+++
categories = ["emacs", "org2blog", "tools", "无所事事"]
comments = true
date = "2011-12-09T22:30:11+08:00"
published = true
status = "publish"
tags = ["emacs", "org2blog", "无所事事", "转载"]
title = "Unlock the McAfee VirusScan Enterprise 8.5i User Interface"
type = "post"
description = ""
+++

[传送门](http://it.megocollector.com/?p=1094)

经检验，只要设置以下注册表就行了:
```conf
[HKEY_LOCAL_MACHINE\SOFTWARE\McAfee\DesktopProtection]
"UIPMode"=dword:00000000
"UIP"=""
```
       
<!--more-->