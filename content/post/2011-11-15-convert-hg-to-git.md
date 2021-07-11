+++
categories = ["emacs", "org2blog", "programming", "tools"]
comments = true
published = true
date = "2011-11-15T10:10:10+08:00"
status = "publish"
tags = ["DVCS", "emacs", "git", "hg", "org2blog", "VCS"]
title = "将hg转换为git"
type = "post"
description = ""
+++

首先安装[fast-export](http://repo.or.cz/w/fast-export.git)

基本命令: 

```sh
cd somedir
git init .
/path/to/fast-export -r /path/to/hg/repo
git reset HEAD
git checkout .
git remote add origin user@server/repo.git
git push origin master
```
      
That's all! 
<!--more-->