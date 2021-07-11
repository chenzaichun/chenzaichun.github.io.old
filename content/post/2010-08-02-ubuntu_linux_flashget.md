+++
categories = ["Linux", "tools", "无所事事"]
comments = true
date = "2010-08-02T17:32:37+08:00"
published = true
status = "publish"
tags = ["Linux", "无所事事"]
title = "ubuntu下使用flashget"
type = "post"
description = ""
+++


按照官方说明安装:

``` 
Ubunt 10.04下面无法启动,提示找不到libexpat.so.0
  error while loading shared libraries: libexpat.so.0: cannot open shared object file: No such file or directory
  原因,编译flashget使用的是libexpat.so.0版本,系统默认为libexpat.so.1.5.2,做软连接即可.
  $cd /usr/lib
  $sudo ln -s libexpat.so.1.5.2 libexpat.so.0
  $sudo ldconfig
```

但是会出现:

``` 
flashget: error while loading shared libraries: libexpat.so.0: wrong ELF class: ELFCLASS64
```

原因是因为我使用的64为系统,而flashget是用32位程序.怎么办呢?因为装有lib32,所以可以直接创建lib32中libexpat.so的软链接

``` 
sudo ln -s /lib32/libexpat.so.1.5.2 /usr/lib/libexpat.so.0
```
<!--more-->