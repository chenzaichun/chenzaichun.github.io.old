+++
categories = ["Linux", "tools"]
comments = true
date = "2010-05-10T23:11:13+08:00"
published = true
status = "publish"
tags = ["archlinux", "Linux"]
title = "archlinux安装yaourt"
type = "post"
description = ""
+++


参照arch wiki：<a href="http://wiki.archlinux.org/index.php/Yaourt_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)">http://wiki.archlinux.org/index.php/Yaourt_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)</a>
 
最简单安装Yaourt的方式是添加Yaourt源至您的 /etc/pacman.conf:

i686架构：

```conf
[archlinuxfr]
Server = http://repo.archlinux.fr/i686
```

x86-64架构：

```conf
[archlinuxfr]
Server = http://repo.archlinux.fr/x86_64
```

同步并安装：

```sh
pacman -Syu
pacman -S yaourt
```

<!--more-->