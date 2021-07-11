+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2011-01-16T19:34:31+08:00"
status = "publish"
tags = ["DVCS", "hg", "mercurial", "python"]
title = "hg certificate warnings"
type = "post"
description = ""
+++

升级到mercurial 1.7.3之后，频繁会遇到像这样的错误：

``` 
warning: bitbucket.org certificate not verified (check web.cacerts config setting)
```

解决办法，参考链接：<a href="http://mercurial.selenic.com/wiki/CACertificates">http://mercurial.selenic.com/wiki/CACertificates</a>

下载<a href="http://curl.haxx.se/ca/cacert.pem" target="_blank">http://curl.haxx.se/ca/cacert.pem</a>到本地，然后在～/.hgrc里面添加这样一句：

```conf
[web]
cacerts=/path/to/cacert.pem
```

或者直接禁用：

```conf
[web]
cacerts =
```
<!--more-->