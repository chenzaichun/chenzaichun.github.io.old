+++
categories = ["Linux", "tools", "无所事事"]
comments = true
published = true
date = "2010-08-08T17:37:32+08:00"
status = "publish"
tags = ["Linux", "python"]
title = "安装fuse-python出现错误"
type = "post"
description = ""
+++

错误为:

``` 
NameError: name 'cflags' is not defined
```

解决办法:

```sh
sudo apt-get install libfuse-dev libfuse2 fuse-utils python-dev
```
<!--more-->