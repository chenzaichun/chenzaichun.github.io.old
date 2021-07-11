+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
date = "2011-04-24T19:34:31+08:00"
status = "publish"
tags = ["archlinux", "Linux", "mingw"]
title = "archlinux下使用MinGW交叉编译testdisk"
type = "post"
description = ""
+++


前面已经配置好了MinGW的交叉编译环境，现在就可以在archlinux下编译testdisk了。由于testdisk需要curses的支持，可以使用PDcureses。

安装PDCurses

```sh
yaourt -S mingw32-pdcurses
```

编译testdisk

```sh
./configure --host=i486-mingw32 --disable-missing-uuid-ok && make
```

这样就编译好了。在windows下运行发现没有问题。:) （终于可以不用忍受windows下mingw的蜗牛速度了）
<!--more-->