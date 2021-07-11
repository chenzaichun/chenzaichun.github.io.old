+++
title = "VKVideoPlayer爬坑记"
tags = ["programming", "objc", "iOS", "video"]
categories = ["programming"]
date = "2015-08-18T14:33:00+08:00"
toc = true
feature = "media/vkvideoplayer-demo.png"
description = ""
+++


# 前言

在 iOS 开发过程中，视频播放相对来说还是比较简单，可以是用`MPMoviePlayerController`，`MPMoviePlayerViewController`, 或者 iOS8中引入的`AVPlayerViewController`，如果要更底层一点，也可以使用`AVKit`来做, 更极端的方式就是使用 `WebView` 来加载视频。

这里简单说明一下他们的区别:

* `MPMoviePlayerController`和`MPMoviePlayerViewController`使用起来类似，不足之处就是不能自定义控件，在使用`HDMI`来投影屏幕的时候会出现很多莫名其妙的问题，同时在 iOS9中将被标记为`deprecated`并不再维护
* `AVPlayerViewController`使用起来也很方便，但是在 iOS8中同样有 HDMI 投影问题
* `AVKit` `AVPlayer`自己实现，比较麻烦
* `WebView`加载使用起来也很方便，HDMI投影也有问题

最近想写一个简单的 Demo，需要播放视频，同时需要自定义播放控件，上面的都不满足条件，网上很多人说`VKVideoPlayer`很不错，查看了一下[VKVideoPlayer](https://github.com/viki-org/VKVideoPlayer), 貌似 feature 特别多：

<!-- more -->

# VKVideoPlayer

## VKVideoPlayer的Feature

Some of the advance features are:

- Fully customizable UI
- No full screen restrictions (have it any size and position you would like!)
- Display subtitles (SRT supported out of the box)
- Customize subtitles (use CSS for styling courtesy of DTCoreText)
- Supports HTTP Live streaming
- Orientation change support (even when orientation lock is enabled)
- Bulletproof event machine to easily integrate features like video ads
- Lots of delegate callbacks for your own logging requirements

看上了这一点`Fully customizable UI`, 这就是我想要的, {% ruby 哈哈|矫情 %}

## 测试VKVideoPlayer Demo

直接 clone 代码来试试看：

```sh
git clone git@github.com:viki-org/VKVideoPlayer.git

pod install

open VKVideoPlayer.xcworkspace
```

编译Demo, 因为使用xcode 7 beta的缘故，iOS9默认是禁用 http 访问的（貌似这样比较{% ruby 操蛋|安全 %}），运行的时候会报错。

> 2015-08-18 14:58:22.059 VKVideoPlayer[2845:41325] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.

解决办法, 修改`Supporting Files`下`VKVideoPlayer-Info.plist`, 加入`NSAppTransportSecurity` dictionary，添加一个 bool 类型的 key `NSAllowsArbitraryLoads`, 设置为`YES`

![iOS 9 HTTP Load](/media/ios9-http-load.png)

{% alert success %}所有使用iOS9编译的工程，默认都要这样添加才能使用http。{% endalert %}

重新编译运行，默认界面有黑边，而且竟然在横屏和竖屏切换情况下layout有问题，

![VKVideoPlayer Layout Issue on iOS 9](/media/vkvideoplayer-layout-issue-ios9.png)

原因在于`VKVideoPlayer`是为 iOS5设计的，并没有使用`Size Classes`和`Auto Layout`。{% ruby 不用管|没办法 %}


## 使用VKVideoPlayer

创建一个工程`vkplayertest`, 创建`Podfile`


```
inhibit_all_warnings!
workspace 'vkplayertest'

pod "VKVideoPlayer", "~> 0.1.1"
```

安装依赖

```sh
pod install

open vkplayertest.xcworkspace
```

在View中添加一个按钮并绑定Action来播放视频：

```objc
#import "VKVideoPlayerViewController.h"

- (IBAction)playVideo:(id)sender {
    VKVideoPlayerViewController *viewController = [[VKVideoPlayerViewController alloc] init];
    [self presentModalViewController:viewController animated:YES];
    [viewController playVideoWithStreamURL:[NSURL URLWithString:@"http://baobab.cdn.wandoujia.com/14394406774325.mp4"]];
}
```

layout问题在多次切换屏幕方向再一次出现：

![VKVideoPlayer Layout Issue](/media/vkvideoplayer-layout-issue-2.png)

{% textcolor danger %}难道是我打开方式不对？{% endtextcolor %} 

## xcode 6测试

用`xcode 6`来测试一下试试看。结果是一样的，将模拟器换成iPhone4S，结果还是一样。

不折腾，果断放弃之。有空自己用`AVPlayer`实现一下。



<!--more-->