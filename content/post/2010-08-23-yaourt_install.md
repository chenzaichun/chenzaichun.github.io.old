+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-08-23T23:17:11+08:00"
tags = ["archlinux", "Linux"]
title = "安装yaourt遇到failed retrieving yaourt"
type = "post"
description = ""
+++

添加源：

```conf
[archlinuxfr]
Server = http://repo.archlinux.fr/x86_64
```

安装x86_64版本的yaourt的时候，出现failed retrieving yaourt，解决办法：

```sh
pacman -Syy yaourt
```
<!--more-->