+++
categories = ["programming"]
comments = true
published = true
status = "publish"
date = "2011-07-01T19:56:31+08:00"
tags = ["ios"]
title = "iOS获取UIWebView当前浏览页面的地址"
type = "post"
description = ""
+++

iOS下获取UIWebView当前浏览页面的地址，在 =-(void) webViewDidFinishLoad:(UIWebView *)wv=

```objc
text = wv.request.URL.absoluteString;
```
<!--more-->