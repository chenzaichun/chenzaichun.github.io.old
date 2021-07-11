+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-08-24T23:23:33+08:00"
tags = ["archlinux", "Linux"]
title = "archlinux console分辨率"
type = "post"
description = ""
+++

archlinux在启动的时候grub使用的是800x600，修改为1280x800时没有这个选项，google得知加上启动vga参数865就可以了

```conf
vga=865
```
<!--more-->