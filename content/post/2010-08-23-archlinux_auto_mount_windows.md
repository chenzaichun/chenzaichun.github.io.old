+++
categories = ["Linux", "tools"]
comments = true
published = true
date = "2010-08-03T18:32:31+08:00"
status = "publish"
tags = ["archlinux", "Linux"]
title = "archlinux中自动挂载windows分区"
type = "post"
description = ""
+++


首先安装ntfs-3g

```sh
sudo pacman -S ntfs-3g
```

然后修改/etc/fstab，比如我的修改为

``` 
/dev/sda1 /media/c vfat user,defaults,codepage=936,iocharset=utf8,umask=000 0 0
/dev/sda5 /media/d ntfs user,defaults,codepage=936,iocharset=utf8,umask=000 0 0
/dev/sda6 /media/e ntfs user,defaults,codepage=936,iocharset=utf8,umask=000 0 0
/dev/sda7 /media/f ntfs user,defaults,codepage=936,iocharset=utf8,umask=000 0 0
```
<!--more-->