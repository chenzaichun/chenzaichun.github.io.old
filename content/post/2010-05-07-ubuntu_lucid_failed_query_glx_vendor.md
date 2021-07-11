+++
categories = ["Linux"]
comments = true
date = "2010-05-07T09:11:33+08:00"
published = true
status = "publish"
tags = ["GLX", "nvidia", "ubuntu"]
title = "ubuntu 10.04 beta Fail to query the GLX server vendor"
type = "post"
description = ""
+++


```sh
unlink /usr/lib/libGL.so
unlink /usr/lib/libGL.so.1
unlink
/usr/lib64/libGL.so
unlink /usr/lib64/libGL.so.1

ln -s /usr/lib/nvidia-current/libGL.so /usr/lib/libGL.so.1
ln -s /usr/lib64/nvidia-current/libGL.so /usr/lib64/libGL.so.1
```
<!--more-->