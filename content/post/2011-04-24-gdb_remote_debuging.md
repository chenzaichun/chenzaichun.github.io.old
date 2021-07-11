+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
date = "2011-04-24T19:44:31+08:00"
status = "publish"
tags = ["archlinux", "gdb", "Linux"]
title = "GDB远程调试"
type = "post"
description = ""
+++

参考链接：<a href="http://blog.csdn.net/fertiland/archive/2008/01/01/2007533.aspx">http://blog.csdn.net/fertiland/archive/2008/01/01/2007533.aspx</a>

前面已经做好跨平台的交叉编译的工作，如果需要调试怎么办呢？这个时候就可以GDB的远程调试来实现。

P.S. 如果没有windows的GDB，可以去<a href="http://www.gnu.org/software/gdb/" target="_blank">下载</a>或者自行编译。
在windows端执行 

```sh
gdbserver localhost:2345 photorec.exe
```

在archlinux下执行

```sh
# 这里我用的virtualbox，所以ip为10.0.2.2，请自行替换成windows的ip
$gdb photorec.exe
gdb> target remote 10.0.2.2:2345
gdb > ....
```
<!--more-->