+++
title = "通过git将bitbucket当做无限容量的dropbox"
date = "2012-09-26T15:47:00+08:00"
comments = true
categories = ["tools", "programming"]
tags = ["git", "dvcs", "vcs"]
description = ""
+++


网上找一个网盘很麻烦，一般都有容量限制，要不就是有诸多限制。有没有geek一点的方法，使用git和bitbucket来作为网盘呢？

google了一下找到了[这个](http://bioinfoblog.it/2011/10/bitbucket-sparkleshare-home-made-limitless-dropbox/)

`git` --> 版本管理

`bitbucket` --> 无限容量的仓库

1. 安装[Sparkshare](http://sparkleshare.org/)
2. 创建一个private的repo
3. 设置ssh key
4. sync

<!--more-->