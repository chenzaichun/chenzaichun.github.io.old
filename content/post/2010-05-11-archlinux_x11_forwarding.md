+++
categories = ["Linux", "tools"]
comments = true
date = "2010-05-11T13:11:32+08:00"
published = true
status = "publish"
tags = ["archlinux", "Linux"]
title = "archlinux x11 forwarding"
type = "post"
description = ""
+++


默认情况下archlinu是不允许x11 forwarding的,修改/etc/ssh/sshd_config

```conf
AllowTcpForwarding yes
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost yes
```
<!--more-->