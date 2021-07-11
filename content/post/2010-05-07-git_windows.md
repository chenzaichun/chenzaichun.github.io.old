+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
status = "publish"
tags = ["DVCS", "git", "Windows"]
title = "git windows"
date = "2010-05-07T11:11:11+08:00"
description = ""
+++


在测试过很多中dvcs之后，最后决定用git。git在linux表现是非常完美的，不管是中文文件名，还是log，同时git svn clone也相当的完美。但是在windows下，乱码问题一直得不到解决。万般无赖之下只有抛弃以前的历史，直接svn co下来之后，生成一个新的git repo。

要想中文显示正常，在msysgit的安装目录下的/etc/git-completion.bash中增加一行：

```
alias ls='ls --show-control-chars --color=auto'
```
修改`/etc/inputrc`中对应的行：

```
alias ls='ls --show-control-chars --color=auto'
```

在`/etc/profile`中增加一行：

```
alias ls='ls --show-control-chars --color=auto'
```

windows下完美解决问题。可惜windows下的repo在linux下是不能够正常使用的！等待解决git官方utf-8支持吧
<!--more-->