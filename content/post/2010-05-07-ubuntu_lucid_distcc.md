+++
categories = ["Linux"]
comments = true
date = "2010-05-07T12:11:33+08:00"
published = true
status = "publish"
tags = ["distcc", "Linux", "ubuntu"]
title = "ubuntu 10.04 lucid安装distcc"
type = "post"
description = ""
+++


在ubuntu下安装distcc（后面两个可选）：

```sh
sudo apt-get install distcc distccmon-gnome distcc-pump
```

设置环境变量:

```sh
export PATH="/usr/lib/distcc:$PATH"
```

打开`/etc/default/distcc`进行以下修改

```conf
STARTDISTCC="true"    # auto start
ALLOWEDNETS="192.168.39.0/24"    # ip range allowed
LISTENER=""   # if use on intranet, must be empty, otherwise distcc: connection refused
ZEROCONF="true"  # auto lookup distcc server
```

然后重启distcc服务

```sh
sudo /etc/init.d/distcc restart
```

搞定！:-)
<!--more-->