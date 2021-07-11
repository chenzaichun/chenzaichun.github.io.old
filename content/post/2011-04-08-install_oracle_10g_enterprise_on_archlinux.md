+++
categories = ["Linux", "oracle", "programming", "tools"]
comments = true
date = "2011-04-08T19:24:31+08:00"
published = true
status = "publish"
tags = ["archlinux", "database", "Linux", "oracle"]
title = "archlinux下安装oracle 10g enterprise"
type = "post"
description = ""
+++

按照arch wiki做就可以了。参考链接<a href="https://wiki.archlinux.org/index.php/Oracle">https://wiki.archlinux.org/index.php/Oracle</a>

这里，安装ksh的步骤可以跳过，直接安装pdksh

```sh
sudo pacman -S pdksh
```

安装过程中可能在62%左右提示一个错误，点continue不影响使用。

默认安装完成之后，重新启动archlinux之后，oracle是不会启动的。手动启动的方法：

<ol>
<li>su - oracle # 这里得以oracle启动</li>
<li>sqlplus /nologo</li>
<li>sql> conn / as sysdba</li>
<li>sql> startup</li>
<li>sql> exit</li>
<li>lsnrctl start</li>
</ol>
<!--more-->