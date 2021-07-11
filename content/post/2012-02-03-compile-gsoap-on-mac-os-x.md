+++
categories = ["emacs", "org2blog", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2012-02-03T23:32:11+08:00"
tags = ["emacs", "macos", "org2blog", "soap", "web service", "xml"]
title = "MacOS X上编译gSOAP"
type = "post"
description = ""
+++


```sh
automake --ignore-deps && ./configure && make
```

如果要编译universal版本，则使用

```sh
automake --ignore-deps && ./configure CFLAGS="-arch ppc -arch i386" LDFLAGS="-arch ppc -arch i386" && make
```
   
<!--more-->