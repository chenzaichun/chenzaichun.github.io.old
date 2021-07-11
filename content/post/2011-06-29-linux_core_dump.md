+++
categories = ["Linux", "tools"]
comments = true
published = true
date = "2011-06-29T19:44:31+08:00"
status = "publish"
tags = ["Linux"]
title = "linux下生成core dump文件"
type = "post"
description = ""
+++


linux下让crash的程序在程序可执行文件目录自动生成core文件：

```sh
ulimit -c unlimited
```
<!--more-->