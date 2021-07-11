+++
categories = ["tools", "无所事事"]
comments = true
date = "2010-09-05T17:32:36+08:00"
published = true
status = "publish"
tags = ["chrome", "Google", "无所事事"]
title = "原来chromium不支持youtube html5"
type = "post"
description = ""
+++


今天用chromium打开youtube用html5播放，提示浏览器不支持，可是原来用chrome的时候是可以直接看的。google发现，chromium没有带解码，无奈之下只好重新安装chrome。

```sh
yaourt -S google-chrome-dev
```

幸好aur中用chrome dev channel
<!--more-->