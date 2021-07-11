+++
categories = ["tools", "无所事事"]
comments = true
date = "2010-12-19T19:14:31+08:00"
published = true
status = "publish"
tags = ["chrome", "Google"]
title = "chrome for cause作弊码"
type = "post"
description = ""
+++


没有想到的是竟然真的有人去弄了个作弊码。由于对js不熟，不知道到底有没有什么副作用。木哈哈

```javascript
javascript:var i=0,loop=setInterval(function(){var newwin=window.open("http://www.google.com");setTimeout(function(){newwin.close();},500);if(++i>=250) clearInterval(loop);},500);void 0;
```
<!--more-->