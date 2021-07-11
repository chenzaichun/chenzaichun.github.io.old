+++
categories = ["Linux", "programming", "tools", "无所事事"]
comments = true
published = true
status = "publish"
date = "2010-07-27T11:23:22+08:00"
tags = ["android", "Linux"]
title = "ubuntu下adb驱动问题"
type = "post"
description = ""
+++


ubuntu下adb默认无法识别,得到error: insufficient permissions for device,使用adb devices得到no permissions.解决办法有两种:

1. 使用root账户(su到root账户)

```sh
su
cd android-tools-dir
./adb kill-server
./adb start-server
./adb devices
```

2. 修改udev的权限,<a href="http://bradchow.wordpress.com/2009/02/16/adb-on-windows-and-ubuntu-linux/" target="_blank">猛击这里</a>,添加/etc/udev/rules.d/50-android.rules

```conf
SUBSYSTEM=="usb", SYSFS(idVendor)=="18d1", MODE="0666"
```

然后修改执行权限

```sh
chmod a+rx /etc/udev/rules.d/50-android.rules
```

重启udev

```sh
sudo restart udev
```

```sh
su
cd android-tools-dir
./adb kill-server
./adb start-server
./adb devices
```
<!--more-->