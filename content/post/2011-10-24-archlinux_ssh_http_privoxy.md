+++
categories = ["emacs", "Linux", "org2blog"]
comments = true
published = true
date = "2011-10-24T06:10:11+08:00"
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "ArchLinux下配置ssh/http代理"
type = "post"
description = ""
+++


被墙煎的滋味确实很难受，ssh转发大家的方法还是比较方便的。

```sh
ssh -C -f -N -g -D port user@Server
```

但是很多的软件并不一定支持sock5代理，这个时候就需要将sock5代理转化为http代理。ArchLinux下的配置安装方法为：

1. 安装Privoxy

```sh
sudo pacman -S privoxy
```

2. 修改`/etc/privoxy/config` , 添加以下语句

```conf
forward-socks5   /               127.0.0.1:sock_port .
listen-address  0.0.0.0:http_port
```

3. 重新启动privoxy 

```sh
sudo /etc/rc.d/privoxy restart
```

Bingo! Enjoy it:) 
<!--more-->