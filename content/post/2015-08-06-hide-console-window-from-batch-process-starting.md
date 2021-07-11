+++
title = "bat 中启动进程并隐藏窗口"
tags = ["programming"]
categories = ["programming"]
date = "2015-08-06T13:08:00+08:00"
description = ""
+++


这几天在将产品移植到 Windows 过程中需要需要启动很多服务进程, 但是需要这么多 console 窗口很难看，所以需要在启动的过程中隐藏 console 窗口，后来在 stackoverflow 上找到了[cmdow](http://www.commandline.co.uk/cmdow/) 这个东东（关键协议还是 MIT），可以通过一下方式启动进程并隐藏窗口。

```
cmdow /run /hid <process_to_start>
```
<!--more-->