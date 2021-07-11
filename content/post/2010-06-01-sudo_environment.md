+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
tags = ["Linux", "ubuntu"]
title = "sudo环境变量"
type = "post"
date = "2010-06-01T11:43:33+08:00"
description = ""
+++


由于sudo运行程序不继承当前用户的环境变量，所以不能使用export var=value； sudo command的方式执行。解决办法：


```sh
sudo var=value command
```
<!--more-->