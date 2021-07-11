+++
title = "使用emacs打开大文件"
date = "2012-11-26T08:48:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["emacs"]
description = ""
+++


经常使用`emacs`来查看日志文件，当日至文件特别大的时候，`emacs`的反应速度实在是不敢恭维。

网上查了一下，有两个有趣的elisp，分享一下：

1. http://www.cnblogs.com/yangyingchao/archive/2011/09/20/2182176.html
2. http://www.emacswiki.org/emacs/vlf.el

`logviewer-mode` 是针对log4j格式优化的，带有高亮显示，带有文件分块读取

`vlf` 则是将文件分块读取。

于是乎我尝试了一下 `logviewer-mode`, 感觉很不错的。




<!--more-->