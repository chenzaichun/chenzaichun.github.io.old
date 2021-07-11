+++
categories = ["programming"]
comments = true
published = true
date = "2010-11-29T23:33:11+08:00"
status = "publish"
tags = ["vs", "Windows"]
title = "使用res协议打开exe里面资源文件"
type = "post"
description = ""
+++

参考：<a href="http://www.cnblogs.com/dazhong/archive/2008/06/16/720507.html">http://www.cnblogs.com/dazhong/archive/2008/06/16/720507.html</a>

```cpp
GetModuleFileName(NULL, szModule, sizeof(szModule)/sizeof(TCHAR));
_stprintf(szFileURL, _T("res://%s/%d"), szModule, IDR_HTML1);
```

但是他们都没有说清楚，最主要的一点就是上面一定要用IDR_HTML1（即资源id），如果使用资源名称，则始终都是不能访问的。
<!--more-->