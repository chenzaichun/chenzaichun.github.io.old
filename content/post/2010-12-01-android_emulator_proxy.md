+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-01T17:32:37+08:00"
status = "publish"
tags = ["android", "Google"]
title = "android模拟器使用代理"
type = "post"
description = ""
+++

网上找了很多，最后总结一下最简单的方法:

```sh
emulator -avd devicename -http-proxy "http://proxyname:port"
```
<!--more-->