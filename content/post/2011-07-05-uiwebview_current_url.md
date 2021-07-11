+++
categories = ["programming"]
comments = true
published = true
status = "publish"
date = "2011-07-05T20:56:31+08:00"
tags = ["ios"]
title = "UIWebView获取当前加载的URL"
type = "post"
description = ""
+++


实现delegate: shouldStartLoadWithRequest

```objc
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType 
{
	NSURL* url = [request  URL];
	self.urlEdit.text = [url absoluteString];
	return YES;
}
```
<!--more-->