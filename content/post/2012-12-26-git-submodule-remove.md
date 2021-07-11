+++
title = "删除git submodule"
date = "2012-12-26T14:02:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["emacs", "linux", "archlinux", "vcs", "dvcs", "git"]
description = ""
+++


git并没有提供删除submodule的方法，只有手动删除。

1. 删除`.gitmodules`下submodule的信息
2. 删除`.git/config`下submodule的信息
3. git rm --cache <submodule/path>
<!--more-->