+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2011-06-25T19:44:31+08:00"
tags = ["DVCS", "hg", "mercurial"]
title = "hg合并多个repository"
type = "post"
description = ""
+++


假如我已经有了一个repo A，并且其中有了一些change set。现在有另外一个repo B，同时里面也有一些change set。现在我想把B移到A中的一个目录，并且保留所有的change set。在mercurial中应该怎么办呢？

1. 通过pull -f的方式

```sh
hg clone A Adir
cd Adir
hg pull -f B
hg merge
hg commit
```

2. 通过subrepo的方式，传送门<a href="http://mercurial.selenic.com/wiki/Subrepository">http://mercurial.selenic.com/wiki/Subrepository</a>

这里第一种方法适用，第二种方法适用与子项目或者库的情况.
<!--more-->