+++
categories = ["emacs", "org2blog", "programming", "tools"]
comments = true
date = "2011-11-16T11:11:11+08:00"
published = true
status = "publish"
tags = ["DVCS", "emacs", "git", "org2blog", "VCS"]
title = "git合并多个repositories到子目录"
type = "post"
description = ""
+++


在每个repo都很小的情况下，维护多个repo是很麻烦的事情。如何在保留原来版本信息的情况下将repo合并到另外一个repo的子目录下呢？有很多的方法，比如在不删除原来的repo，使用submodule。或者使用subtree的方式。但是我不想用submodule保留原来的repo，而且感觉subtree也是一个比较麻烦的东东。这里要介绍git的read-tree（可以参考[这里](http://blog.ossxp.com/2010/10/2041/))。   网上讲了很多，我总结一下并写了一个脚本repomerge.sh。先把脚本贴出来 

```sh
#!/bin/sh

git remote add -f $1 $2
git merge -s ours --no-commit $1/master
git read-tree --prefix=$1/ -u $1/master
git commit -m "merging $1 into subdirectory"
```
      
使用的方法是$1为子目录名字，$2为repo的路径。

这里有一个问题需要注意，合并到的repo（即目标repo）不能为空（至少要有一个commit)。

比如要合并一个repo A 到A目录，repo B到B目录 

```sh
cd dest-repo-dir
git init .

# add one file and commit
touch just-test-file
git add just-test-file
git commit -am "init"

/path/to/repomerge.sh A /path/to/repo/A
/path/to/repomerge.sh B /path/to/repo/B
```
      
这样repo A和repo B就分别被合并到了A、B目录了。 
<!--more-->