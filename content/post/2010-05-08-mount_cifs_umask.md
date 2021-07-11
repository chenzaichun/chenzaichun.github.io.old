+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-05-08T18:23:34+08:00"
tags = ["Linux", "samba"]
title = "Mount.cifs 中设置文件权限"
type = "post"
description = ""
+++

在以往常用的Mount命令里，使用`umask=xxxx`设置挂载目录文件的权限，然后在使用Smbfs的Mount工具时，便不是使用umask了，而是用

```conf
-o file_mode=0666 dir_mode=0755
```

这个问题纠结了我半天，终于在Google上找到答案
<!--more-->