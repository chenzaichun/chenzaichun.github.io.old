+++
title = "Mac OS X下安装gevent"
date = "2013-06-23T11:08:00+08:00"
comments = true
categories = ["programming"]
tags = ["tools", "python", "macosx"]
description = ""
+++


(请确保xcode command line tools已经安装)

直接使用 =pip install= 会出现 =event.h= 找不到：

```
   clang: warning: argument unused during compilation: '-mno-fused-madd'
    In file included from gevent/core.c:253:
    gevent/libevent.h:9:10: fatal error: 'event.h' file not found
    #include "event.h"
             ^
    1 error generated.
    error: command 'clang' failed with exit status 1
```

这个时候需要安装 =libevent=:

```
brew install libevent
```

然后安装 =gevent=
```
sudo pip install gevent
```

安装成功之后，测试一下发现会出错：

```
>>> import gevent
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Library/Python/2.7/site-packages/gevent/__init__.py", line 40, in <module>
ImportError: cannot import name core
```

原来是因为没有安装 =cython=, 安装之

```
sudo pip install cython
```

并重新安装 =greenlet= 和 =gevent=
```
sudo pip uninstall gevent
sudo pip uninstall greenlet
sudo pip install greenlet
sudo pip install gevent
```

<!--more-->