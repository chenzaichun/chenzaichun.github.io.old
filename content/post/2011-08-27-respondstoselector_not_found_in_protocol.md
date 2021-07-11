+++
categories = ["ios", "programming"]
comments = true
published = true
status = "publish"
date = "2011-08-27T20:56:31+08:00"
tags = ["ios"]
title = "'-respondsToSelector:' not found in protocol(s)"
type = "post"
description = ""
+++

 
The respondsToSelector: method is declared in the NSObject protocol. You have to make sure that your custom protocols also conform to the NSObject protocol. Change the declarations of your custom protocols from:

```objc
@protocol MyCustomProtocol...@end
```

To:

```objc
@protocol MyCustomProtocol <NSObject>...@end
```
<!--more-->