+++
categories = ["emacs", "Linux", "org2blog", "tools"]
comments = true
date = "2011-11-17T11:11:11+08:00"
published = true
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "常用软件的代理设置"
type = "post"
description = ""
+++


```sh
# http_proxy
export http_proxy=host:port

# https_proxy
export https_proxy=host:port

# ftp_proxy
export ftp_proxy=host:port

# rsync proxy
export RSYNC_PROXY=host:port
```
```conf
# ssh over http_proxy depends on your tools used.
# Modify .ssh/config file for host configuration:
# Following are connect.c and corkscrew example.
# For connect.c, -h option means use http_proxy
# environment. 
Host *
  ProxyCommand corkscrew http-proxy.example.com 8080 %h %p
  ProxyCommand connect -h %h %p
  ProxyCommand connect -H proxy-host:proxy-port %h %p
```

```
# git over http_proxy
export GIT_PROXY_COMMAND="connect -h $@"
```
<!--more-->