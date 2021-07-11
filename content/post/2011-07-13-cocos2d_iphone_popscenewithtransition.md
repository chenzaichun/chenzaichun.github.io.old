+++
categories = ["ios", "programming"]
comments = true
date = "2011-07-13T20:56:31+08:00"
published = true
status = "publish"
tags = ["ios"]
title = "cocos2d-iphone popSceneWithTransition"
type = "post"
description = ""
+++


原文链接：<a href="http://www.cocos2d-iphone.org/forum/topic/1076" target="_blank">http://www.cocos2d-iphone.org/forum/topic/1076</a>
在CCDirector.h添加

```objc
- (void) popSceneWithTransition: (Class)c duration:(ccTime)t;
```

在CCDirector.m中添加


```objc
-(void) popSceneWithTransition: (Class)transitionClass duration:(ccTime)t;
{
	NSAssert( runningScene_ != nil, @"A running Scene is needed");

	[scenesStack_ removeLastObject];
	NSUInteger c = [scenesStack_ count];
	if( c == 0 ) {
		[self end];
	} else {
		CCScene* scene = [transitionClass transitionWithDuration:t scene:[scenesStack_ objectAtIndex:c-1]];
		[scenesStack_ replaceObjectAtIndex:c-1 withObject:scene];
		nextScene_ = scene;
	}
}
```

调用方式

```objc
[[CCDirector sharedDirector] popSceneWithTransition:[CCSlideInRTransition class] duration:0.5f];
```
<!--more-->