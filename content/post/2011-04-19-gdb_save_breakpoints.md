+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
date = "2011-04-19T19:24:31+08:00"
status = "publish"
tags = ["gdb", "Linux"]
title = "gdb保存和加载断点等信息"
type = "post"
description = ""
+++


刚开始使用gdb，在重启gdb的时候断点、环境变量等信息都必须重新设置，感觉很不方便。不过gdb提供了.gdbinit，可以方便的在启动的时候对gdb进行设置。比如下面用于保存和加载breakpoints

``` 
define bsave
  shell rm -f brestore.txt
  set logging file brestore.txt
  set logging on
  info break
  set logging off
  # reformat on-the-fly to a valid gdb command file
  shell perl -n -e 'print "break $1n" if /^d+.+?(S+)$/g' brestore.txt > brestore.gdb
end 
document bsave
store actual breakpoints
end

define brestore
  source brestore.gdb
end
document brestore
restore breakpoints saved by bsave
end
```

调用bsave保存breakpoints，调用bresotre加载breakpoints。
再如对于调试ncurses的设置：

``` 
define settty
  tty /dev/pts/$arg0
end

define setterminfo
  set env TERM=xterm
  set env COLUMNS=144
  set env LINES=29
end
```
<!--more-->