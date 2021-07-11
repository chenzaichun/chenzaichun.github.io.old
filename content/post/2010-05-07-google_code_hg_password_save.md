+++
categories = ["programming"]
comments = true
published = true
date = "2010-05-07T11:11:11+08:00"
tags = ["DVCS", "hg", "mercurial"]
title = "google code hg 保存密码"
description = ""
+++


hg不能像svn那样保存密码，每次都要输入密码不太方便，可以通过

编辑工程目录下`.hg/hgrc`

```
[paths]
default = https://projectname.googlecode.com/hg
```

把密码和用户名填到`URL`里就行啦

```
[paths]
default = https://user:pwd@projectname.googlecode.com/hg
```
<!--more-->