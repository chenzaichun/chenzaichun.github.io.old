+++
categories = ["emacs", "Linux", "org2blog", "programming"]
comments = true
date = "2011-12-01T22:01:33+08:00"
published = true
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "linux下开启coredump"
type = "post"
description = ""
+++


当linux下的程序运行异常时，会在运行程序的目录下生成core dump文件。通过`ulimit -c`查看，结果为0则为关闭（默认值）.

临时开启只要在shell中执行

```sh
ulimit -c unlimited
```

也可以在`~/.profile`文件中添加来默认开启core dump。 
<!--more-->