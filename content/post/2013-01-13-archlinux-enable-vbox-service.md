+++
title = "archlinux下启用virtualbox service"
date = "2013-01-03T17:47:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["emacs", "linux", "archlinux"]
description = ""
+++


安装好 `virtualbox-guest-utils` 之后，没有并没有启用 `vboxservice`, 这个时候如果windows休眠，则archlinux的时间将不会跟host同步，需要启动vboxservice才行。

```sh
systemctl enable vboxservice.service
```

重启之后生效，当然也可以直接启动

```sh
systemctl start vboxservice.service
```

virtualbox也提供了参数来控制同步，详情见[这里](http://www.virtualbox.org/manual/ch09.html#changetimesync)

```sh
VBoxManage guestproperty set “the name of your guest VM” “/VirtualBox/GuestAdd/VBoxService/–timesync-set-threshold” 15000
```
<!--more-->