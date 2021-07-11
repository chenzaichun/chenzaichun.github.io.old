+++
categories = ["emacs", "org2blog"]
comments = true
published = true
date = "2011-11-15T11:11:11+08:00"
status = "publish"
tags = ["emacs", "oracle", "org2blog"]
title = "Archlinux下安装oracle 10gr2 Enterprise"
type = "post"
description = ""
+++


由于Oracle XE精简了很多，跟教科书上很多不一样，所以还是装一个企业版吧。

[参考链接](http://meansonw-work.blogspot.com/2008/10/archlinux-oracle-10g.html)

安装跟archlinux的wiki上差不多，注意两个链接必须创建：

```sh
sudo ln -s /usr/lib/libgcc_s.so.1 /lib/libgcc_s.so.1
sudo ln -s /bin/tr /usr/bin/tr
```
     
<!--more-->