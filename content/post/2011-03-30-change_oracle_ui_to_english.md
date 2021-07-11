+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2011-03-30T19:24:31+08:00"
tags = ["database", "oracle"]
title = "修改oracle的默认界面语言为英文"
type = "post"
description = ""
+++

在注册表中查找`HKEY_LOCAL_MACHINE\SOFTWARE\Oracle<Your Oracle Home>`

设置`NLS_LANG`

```conf
NLS_LANG = AMERICAN_AMERICA.WE8ISO8859P1
```
<!--more-->