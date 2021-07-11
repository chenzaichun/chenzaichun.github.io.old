+++
categories = ["programming"]
comments = true
published = true
date = "2011-07-05T19:56:31+08:00"
status = "publish"
tags = ["ios"]
title = "iOS弹出输入框"
type = "post"
description = ""
+++


最简单的弹出输入框的方法是添加一个UITextField到UIAlertView中：

```objc
UIAlertView *myAlertView = [[UIAlertView alloc] initWithTitle:@"Your title here" message:@"this gets covered" delegate:self cancelButtonTitle:@"Cancel" otherButtonTitles:@"OK", nil];
UITextField *myTextField = [[UITextField alloc] initWithFrame:CGRectMake(12.0, 45.0, 260.0, 25.0)];

CGAffineTransform myTransform = CGAffineTransformMakeTranslation(0.0, 130.0);
[myAlertView setTransform:myTransform];

[myTextField setBackgroundColor:[UIColor whiteColor]];
[myAlertView addSubview:myTextField];
[myAlertView show];
[myAlertView release];
```
<!--more-->