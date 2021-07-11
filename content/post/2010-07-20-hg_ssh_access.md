+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-07-20T17:23:33+08:00"
tags = ["DVCS", "hg"]
title = "hg ssh 访问"
type = "post"
description = ""
+++


hg通过ssh访问时，地址的格式是ssh://user:pwd@host//path,注意这里的两个/，如果只是使用一个，则找不到repo，这个与scp等的方式不一样（scp的格式是：scp user:pwd@host:/path）详情见`hg help urls`

```sh
hg help urls
```
<!--more-->