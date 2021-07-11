+++
categories = ["Linux", "tools"]
comments = true
published = true
date = "2011-03-01T19:24:31+08:00"
status = "publish"
tags = ["archlinux", "Linux"]
title = "yaourt代理设置"
type = "post"
description = ""
+++

一直以来不知道原来yaourt是调用makepkg来实现下载、编译、安装的

``` 
cat /etc/makepkg.conf

DLAGENTS=('ftp::/usr/bin/wget -c --passive-ftp -t 3 --waitretry=3 -O %o %u'
                  'http::/usr/bin/wget -c -t 3 --waitretry=3 -O %o %u'
                  'https::/usr/bin/wget -c -t 3 --waitretry=3 --no-check-certificate -O %o %u'
                  'rsync::/usr/bin/rsync -z %u %o'
                  'scp::/usr/bin/scp -C %u %o')
```

很多时候可能用到了rsync的协议，所以添加一个RSYNC_PROX的环境变量比较保险。
<!--more-->