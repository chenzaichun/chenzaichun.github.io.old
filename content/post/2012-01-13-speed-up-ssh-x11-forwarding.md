+++
categories = ["emacs", "org2blog", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2012-01-13T17:16:14+08:00"
tags = ["emacs", "Linux", "org2blog"]
title = "X11 Forwarding提速"
type = "post"
description = ""
+++


[参考链接](http://www.miscdebris.net/blog/2007/06/01/speed-up-ssh-x11-forwarding/"), 原理就是通过-C压缩传输，-c修改加密算法来提高传输

```sh
ssh -c arcfour,blowfish-cbc -XC host.com
```

通过alias设置:

```sh
alias ssh-x='ssh -c arcfour,blowfish-cbc -XC'
```
<!--more-->