+++
categories = ["Linux", "tools"]
comments = true
published = true
date = "2010-08-29T23:32:33+08:00"
status = "publish"
tags = ["archlinux", "Linux"]
title = "xelatex"
type = "post"
description = ""
+++

今天准备用latex改一下简历，但是原来坚哥配置的texlive已经无法使用（因为squashfs升级，不支持旧版的格式），想了一下，还是装个新版的texlive吧！archlinux下安装：


```sh
sudo pacman -S texlive-core textlive-langcjk texlive-latexextra
```

新版的xelatex对中文支持比较好了，所以直接修改原来用moderncv写的简历，然后xelatex xxx.tex生成pdf，相当的简单！
<!--more-->