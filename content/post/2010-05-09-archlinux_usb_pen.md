+++
categories = ["Linux"]
comments = true
date = "2010-05-09T21:13:43+08:00"
published = true
status = "publish"
tags = ["archlinux", "Linux", "portable"]
title = "将archlinux安装到u盘"
type = "post"
description = ""
+++

将archlinux安装到优盘上！试了很多方法，在虚拟机上将优盘设置成硬盘安装和在虚拟机上设置usb设备安装，启动时都会出现找不到硬盘的情况。（使用虚拟机启动没有问题）解决办法（<a href="http://blog.sina.com.cn/s/blog_59cf67260100cwqr.html">参考地址</a>）：


设置系统的参数的时候，修改/etc/mkinitcpio.conf ，在HOOKS中加入usb选项，这似乎是让linux内核认usb。

```conf 
HOOKS="base udev autodetect pata scsi sata usb filesystems"
```

这样就可以正常启动了！如果出现

``` 
The superblock could not be read or does not describe a correct ext2 filesystem. If the device is valid and it really contains an ext2 filesystem (and not swap or ufs or something else), then the superblock is corrupt, and you might try running e2fsck with an alternate superblock:
e2fsck -b 8193
```

请修改fstab，使用uuid的方式，见<a href="http://wiki.archlinux.org/index.php/Fstab">http://wiki.archlinux.org/index.php/Fstab</a>
<!--more-->