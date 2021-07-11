+++
categories = ["Linux", "tools"]
comments = true
published = true
date = "2010-08-23T12:32:37+08:00"
status = "publish"
tags = ["archlinux", "Linux"]
title = "archlinux+LXDE蓝牙传输文件"
type = "post"
description = ""
+++


首先安装bluez, obexfs

```sh
sudo pacman -S bluez obexfs
```

启动bluetooth（也可以加入/etc/rc.conf的daemon中）

```sh
/etc/rc.d/bluetooth start
```

获取蓝牙的mac地址

```sh
hcitool scan
```

启动另外一个term，然后进行连接，如：

```sh
bluez-simple-agent hci0 00:26:37:45:29:8F
```

在其他term中可以进行文件传输了：

```sh
obexftp -b 00:26:37:45:29:8F -p xxx.txt
```
<!--more-->