+++
categories = ["emacs", "tools"]
comments = true
published = true
status = "publish"
date = "2011-07-03T19:56:31+08:00"
tags = ["emacs", "macos"]
title = "Build Emacs on Mac OS X"
type = "post"
description = ""
+++


emacs.app设置中文字体失败的原因已经找到，默认emacs编译的选项为--cocoa，这里cocoa的字体引擎可能有问题。如果以x的方式自己手动编译emacs (--with-x)的时候，字体的渲染和设置都很正常。（如果大家想尝试x的emacs，可以通过以下方式进行安装)

```sh
brew install emacs --with-x
```

这种方式默认emacs被安装在/usr/local/Cellar/emacs/23.3/目录下。但是以x的方式编译出来的emacs，在mac下不是一般的丑- -！！

在Mac OS X下可以通过auto tools编译emacs。

```sh
brew install emacs --with-x
```

```sh
./configure -with-jpeg=no --with-png=no --with-gif=no --with-tiff=no --with-ns --with-x=no
make
```
<!--more-->