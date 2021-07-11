+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-05-10T13:11:11+08:00"
tags = ["archlinux", "Linux"]
title = "archlinux全局代理"
type = "post"
description = ""
+++


参考: <a href="http://wiki.archlinux.org/index.php/Proxy_settings">http://wiki.archlinux.org/index.php/Proxy_settings</a>

在教育网中使用aur或者abs,网速非常的慢,同时很多东西都不能正常的下载下来,如何给下载添加代理呢? 像wget等程序使用"protocal_proxy"环境变量,所以可以通过设置环境变量的方式来使用代理:

```sh
export http_proxy=http://10.203.0.1:5187/
export ftp_proxy=http://10.203.0.1:5187/
```

我们可以增加两个函数用来打开关闭代理,在.bashrc里面添加:

```sh
function proxy(){
    echo -n “username:”
    read -e username
    echo -n “password:”
    read -es password
    export http_proxy=”http://$username:$password@proxyserver:8080/”
    export ftp_proxy=”http://$username:$password@proxyserver:8080/”
    echo -e “nProxy environment variable set.”
}
function proxyoff(){
    unset HTTP_PROXY
    unset http_proxy
    unset FTP_PROXY
    unset ftp_proxy
    echo -e “nProxy environment variable removed.”
}
```
<!--more-->