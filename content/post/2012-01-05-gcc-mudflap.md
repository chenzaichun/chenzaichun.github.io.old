+++
categories = ["emacs", "Linux", "org2blog", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2012-01-05T16:12:33+08:00"
tags = ["emacs", "Linux", "org2blog"]
title = "gcc mudflap用来检测内存越界的问题"
type = "post"
description = ""
+++

[参考链接](http://blog.yufeng.info/archives/698)

可是我使用这个方法编译运行程序出现了这个问题: 

> mf: erroneous reentrancy detected in `__mf_check'
> Aborted

网上找了很久，未果。还是用`valgrind`吧。 
<!--more-->