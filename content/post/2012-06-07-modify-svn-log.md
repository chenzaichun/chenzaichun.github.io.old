+++
title = "修改SVN中已提交的日志"
date = "2012-06-07T13:30:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["vcs", "svn"]
description = ""
+++


默认情况下，subversion server是不允许客户端修改提交后的任何东西的。如果想修改服务器端的配置，请移步[这里](http://blog.csdn.net/karl_max/article/details/7035866)

在服务器端已经配置好的情况下，可以使用以下命令来修改:

```sh
$svn propedit -r N --revprop svn:log URL 
$svn propset -r N --revprop svn:log "new log message" URL 
```

如果想修改最后提交版本的log，可以使用:

```sh
$ svn propedit -r 'HEAD' --revprop svn:log 
```

这时候svn会读取原来log并打开默认编辑器进行修改。
<!--more-->