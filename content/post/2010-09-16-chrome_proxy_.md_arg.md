+++
categories = ["tools", "无所事事"]
comments = true
published = true
date = "2010-09-16T17:32:33+08:00"
status = "publish"
tags = ["chrome", "Google"]
title = "chrome代理相关命令行总结"
type = "post"
description = ""
+++

chrome关于代理的命令行参数有：

``` 
--proxy-auto-detect --proxy-pac-url=http://xxx.xxx.com  // 使用自动在线pac代理
--proxy-auto-detect --proxy-pac-url=file:///pac-file-path  // 使用本地pac文件自动代理
--proxy-server=127.0.0.1:8000 // 使用http代理
--proxy-server=socks5://127.0.0.1:8000 // 使用socks5代理
```
<!--more-->