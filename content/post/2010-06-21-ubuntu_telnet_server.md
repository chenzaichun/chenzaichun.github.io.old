+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-06-21T23:11:34+08:00"
tags = ["telnet", "ubuntu"]
title = "ubuntu下安装telnet服务器"
type = "post"
description = ""
+++


1. 安装xinetd和telnetd

```sh
sudo apt-get install xinetd telnetd
```

2. /etc/inetd.conf在安装过程中已经配置好，修改/etc/xinetd.conf

```conf
defaults{

# Please note that you need a log_type line to be able to use log_on_success
# and log_on_failure. The default is the following :
# log_type = SYSLOG daemon info
## Please note that you need a log_type line to be able to use log_on_success
## and log_on_failure. The default is the following :## log_type = SYSLOG daemon info
# 
## start the insert content
#
# if I have time, I will add some comments about this part.
# instances =60#log_type = SYSLOG authpriv
# log_on_success = HOST PID
# log_on_failure = HOST
# tcps = 25 30
## end the insert content
```

3. 添加/etc/xinet.d/telnet

```conf
#service telnet
#{
#disable = no
#flags = REUSE
#socket_type = stream
#wait = no
#user = root
#server = /usr/sbin/in.telnetd
#log_on_failure += USERID
#}
```
4. 重启xinetd服务

```sh
sudo /etc/init.d/xinetd restart
```

5. 测试一下

```sh
telnet 127.0.0.1
```
<!--more-->