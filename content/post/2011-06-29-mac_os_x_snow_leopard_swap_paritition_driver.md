+++
categories = ["tools", "无所事事"]
comments = true
published = true
status = "publish"
date = "2011-06-29T19:54:31+08:00"
tags = ["macos"]
title = "Mac OS X Snow Leopard下移动swap到另外的分区"
type = "post"
description = ""
+++

参考链接：<a href="http://superuser.com/questions/28414/moving-the-swapfiles-to-a-dedicated-partition-in-snow-leopard">http://superuser.com/questions/28414/moving-the-swapfiles-to-a-dedicated-partition-in-snow-leopard</a>

这里注意的是，新的swap文件的路径必须存在。

```sh
# backup the file
$ cd /System/Library/LaunchDaemons
$ sudo cp com.apple.dynamic_pager.plist{,_bak}

# convert the file to xml
$ sudo plutil -convert xml1 com.apple.dynamic_pager.plist

# modify the file
$ sudo vim com.apple.dynamic_pager.plist

<key>ProgramArguments</key>
<array>
    <string>/bin/bash</string>
    <string>-c</string>
    <string>/bin/wait4path /Volumes/Swap/ &amp;&amp;
/sbin/dynamic_pager -F /Volumes/Swap/.vm/swapfile</string>
</array> # replace /Volumes/Swap/.vm/swapfile as your path

# convert the file to binary
$ sudo plutil -convert binary1 com.apple.dynamic_pager.plist
```

重启就可以了。
<!--more-->