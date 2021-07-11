+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-05-18T12:12:23+08:00"
tags = ["grub2", "Linux", "ubuntu"]
title = "修改Grub2,XP默认首启动"
type = "post"
description = ""
+++

参考： <a href="http://swsw4321.blog.163.com/blog/static/3245245201003121842841/">http://swsw4321.blog.163.com/blog/static/3245245201003121842841/</a>
修改启动项, 终端输入：

```sh
cd /etc/grub.d && ls
```

显示的文件是这样的：

``` 
00_header  05_debian_theme  10_linux  20_memtest86+ 30_os-prober  40_custom  README
```

其中：10_linux就是当前所使用的操作系统，30_os-prober的作用是自动查找计算机的其他系统，比如是windows xp，要XP默认首启动只要执行命令：

```sh
sudo mv 20_os-prober 06_os-prober，把20_os-prober 改成06_os-prober
sudo mv 30_os-prober 06_os-prober
```

这样，以后grub再次更新时，XP就默认在linux前启动了。

```sh
sudo update-grub
```

看看效果吧，这样以后每次更新grub,都是默认XP启动。
<!--more-->