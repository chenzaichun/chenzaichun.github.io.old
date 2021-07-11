+++
title = "Debian Linux下安装pptp vpn服务器"
date = "2012-07-03T10:03:00+08:00"
comments = true
categories = ["tools"]
tags = ["linux", "programming"]
description = ""
+++


首先更新:

```sh
apt-get update
```

接着我们就安装pptp服务器:

```sh
apt-get install pptpd
```

编辑`/etc/pptpd.conf`, 反注释掉以下内容：

```conf
localip 192.168.0.1
remoteip 192.168.0.234-238,192.168.0.245
```

<!--more-->

编辑`/etc/ppp/chap-secrets`, 添加用户，格式为：

```
# client        server  secret                  IP addresses
yesokay          *      password                *
```

修改dns配置, `/etc/ppp/options`:

```conf
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

启用ip_forward, `/etc/sysctl.conf`:

```conf
net.ipv4.ip_forward=1
```

启用新的配置：

```sh
sysctl -p
```

重启pptp服务器：

```sh
/etc/init.d/pptpd restart
```

最后开启iptables转发

```sh
/sbin/iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth0 -j MASQUERADE
```

当然还有更加方便的，有人做了一个一键安装脚本

1. Debian系见[这里](http://www.8ke.in/soft/pptpandl2tpondebian.sh)
2. RPM系见[这里](http://www.diahosting.com/dload/pptpd.sh)
