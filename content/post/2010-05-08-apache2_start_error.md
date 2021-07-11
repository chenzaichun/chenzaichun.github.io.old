+++
categories = ["tools"]
comments = true
published = true
status = "publish"
date = "2010-05-08T11:12:23+08:00"
tags = ["apache", "Windows"]
title = "Apache 2 启动错误的解决"
type = "post"
description = ""
+++


[参考地址](http://www.ourcampus.cn/blog/?action=show&id=27)

启动Apache服务，测试一下环境是否正确，在浏览器中输入 <a href="http://127.0.0.1/">http://127.0.0.1</a> 或是 <a href="http://localhost/">http://localhost</a> 均半天不出页面，查看Apache状态为启动，估计Apache有问题，于是转到Apache安装目录的logs目录下，发现果然有一个error.log, 打开看到里面的内容如下：

``` 
[Thu Nov 22 15:19:53 2007] [notice] Parent: Created child process 2816
[Thu Nov 22 15:19:53 2007] [notice] Child 2816: Child process is running
[Thu Nov 22 15:19:53 2007] [crit] (OS 10022)提供了一个无效的参数。Child 2816:
setup_inherited_listeners(), WSASocket failed to open the inherited
socket.
```

解决方案如下：

1. 网上邻居->本地连接->属性->Internet协议(TCP/IP)->属性->高级->WINS标签 ->去掉启用LMhosts查询前的勾。

2. 网上邻居->本地连接->属性->Internet协议 (TCP/IP)->属性->高级->WINS标签->NetBOIS设置->禁用 TCP/IP 上的  NetBOIS。

3. 控制面版->Windows防火墙->高级标签->本地连接设置->服务标签->勾 选安全Web服务器(HTTPS)。

按照上面的步骤做完，问题解决。
<!--more-->