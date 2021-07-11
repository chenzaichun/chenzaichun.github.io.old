+++
categories = ["emacs", "org2blog", "tools"]
comments = true
published = true
status = "publish"
date = "2011-12-31T17:31:00+08:00"
tags = ["emacs", "macos", "org2blog"]
title = "让Finder显示所有文件"
type = "post"
description = ""
+++


Finder默认情况是不显示隐藏文件和.开头的文件和目录的，由于很多类unix/Linux的软件的配置文件（目录）是.开头的，有的时候我们想在Finder中使用怎么办呢？ 

开启所有文件的显示：

```sh
defaults write com.apple.finder AppleShowAllFiles -bool true
killall Finder
```

重新开启Finder就能看到所有文件了。

关闭显示隐藏文件：

```sh
defaults write com.apple.finder AppleShowAllFiles -bool false
killall Finder
```
<!--more-->