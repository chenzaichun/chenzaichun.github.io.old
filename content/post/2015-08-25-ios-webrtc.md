+++
title = "编译iOS版本WebRTC"
tags = ["programming", "objc", "iOS", "video"]
categories = ["programming"]
date = "2015-08-25T10:06:00+08:00"
toc = false
#feature = null  # To use: uncomment and replace null with value
description = ""
+++


[`WebRTC`](https://zh.wikipedia.org/wiki/WebRTC)，名称源自网页实时通信（英语：Web Real-Time Communication）的缩写，是一个支持网页浏览器进行实时语音对话或视频对话的API。它于2011年6月1日开源并在Google、Mozilla、Opera支持下被纳入万维网联盟的W3C推荐标准


# iOS中WebRTC参考资料 

- [webrtc官网](http://www.webrtc.org/)
- [webrtc-ios](https://github.com/gandg/webrtc-ios) 不过很近没更新了
- [quickblox](http://quickblox.com/) 集成方案，商用价格还是可以
- [How to get started with WebRTC and iOS without wasting 10 hours of your life](http://ninjanetic.com/how-to-get-started-with-webrtc-and-ios-without-wasting-10-hours-of-your-life/) 这篇博客借鉴意义特别大
- [android ios webrtc编译脚本](https://github.com/pristineio/webrtc-build-scripts)


# Mac下编译WebRTC

准备好梯子, 按照官方教程编译, 参考[这里](http://www.webrtc.org/native-code/ios)

<!-- more -->

```sh
mkdir webrtc_ios && cd webrtc_ios

export GYP_DEFINES="OS=ios"

fetch webrtc_ios
```

慢着，这里的fetch命令貌似没有啊，原来是没有安装[`depot_tools`](http://dev.chromium.org/developers/how-tos/install-depot-tools)

```sh
cd <some_where_for_depot_tools>

git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

export PATH=`pwd`/depot_tools:"$PATH"
```

重新做第一步fetch,

下载了竟6G的东东竟然失败了：

>
> Error: Command '/usr/bin/python -u src/sync_chromium.py --target-revision f8d6ba9efdddfb3aa0dfc01cd579f500a2de0b8d' returned non-zero exit status 2 in /Users/chenza/src/webrtc_ios
> Traceback (most recent call last):
>   File "/Users/chenza/src/depot_tools/fetch.py", line 342, in <module>
>     sys.exit(main())
>   File "/Users/chenza/src/depot_tools/fetch.py", line 337, in main
>     return run(options, spec, root)
>   File "/Users/chenza/src/depot_tools/fetch.py", line 331, in run
>     return checkout.init()
>   File "/Users/chenza/src/depot_tools/fetch.py", line 142, in init
>     self.run_gclient(*sync_cmd)
>   File "/Users/chenza/src/depot_tools/fetch.py", line 76, in run_gclient
>     return self.run(cmd_prefix + cmd, **kwargs)
>   File "/Users/chenza/src/depot_tools/fetch.py", line 66, in run
>     return subprocess.check_output(cmd, **kwargs)
>   File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/subprocess.py", line 573, in check_output
>     raise CalledProcessError(retcode, cmd, output=output)
> subprocess.CalledProcessError: Command '('gclient', 'sync', '--with_branch_heads')' returned non-zero exit status 2

重试几次无果，

```sh
gclient sync
```

会失败，原因是访问google storage失败。

{% ruby 该死|天朝 %}的网络呀。 

挂梯子，全程翻，还是失败。只好使用人家已经编译好的版本。见[`apprt-ios`](https://github.com/ISBX/apprtc-ios), apprt-ios提供了pod，可以直接使用。

<!--more-->