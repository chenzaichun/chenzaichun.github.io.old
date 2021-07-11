+++
categories = ["emacs", "org2blog", "tools"]
comments = true
excerpt = "macos-x-seperate-swap-partition"
published = true
status = "publish"
date = "2011-12-30T12:23:33+08:00"
tags = ["emacs", "macos", "org2blog"]
title = "MacOS X Swap单独分区"
type = "post"
description = ""
+++


[参考链接](http://superuser.com/questions/28414/moving-the-swapfiles-to-a-dedicated-partition-in-snow-leopard)

1. 备份dynamic_pager.plist
```sh
cd /System/Library/LaunchDaemons
sudo cp com.apple.dynamic_pager.plist{,_bak}
```

2. 将plist转换为xml格式 
```sh
sudo plutil -convert xml1 com.apple.dynamic_pager.plist
```

<!--more-->

3. 编辑plist文件, 比如我的，请根据自身情况修改路径
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs$
<plist version="1.0">
<dict>
    <key>EnableTransactions</key>
    <true/>
    <key>HopefullyExitsLast</key>
    <true/>
    <key>Label</key>
    <string>com.apple.dynamic_pager</string>
    <key>OnDemand</key>
    <false/>
    <key>ProgramArguments</key>
    <array>
         <string>/bin/bash</string>
         <string>-c</string>
         <string>/bin/wait4path /Volumes/Swap/ &amp;&amp; /sbin/dynamic_pager -F /Volumes/Swap/.vm/swapfile</string>
    </array>
</dict>
</plist>
```

4. 将plist转换为binary
```sh
sudo plutil -convert binary1 com.apple.dynamic_pager.plist
```

搞定 
