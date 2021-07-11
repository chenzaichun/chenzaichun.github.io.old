+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-06-07T23:33:11+08:00"
tags = ["Qt", "Windows"]
title = "windows下使用incredibuild编译Qt"
type = "post"
description = ""
+++


默认configure的时候qt的编译程序会检测incredibuild的xge支持，所以如果安装了incredibuild的话，会检测到xge选项，而configure的时候得到using incredibuildxge --yes
 

```sh
configure -no-qt3support -no-webkit -release -opensource -platform win32-msvc2008
```

完成configure之后，会在根目录生成一个projects.sln，这个时候就可以使用incredibuild编译了。
<!--more-->