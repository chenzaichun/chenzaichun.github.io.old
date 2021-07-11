+++
categories = ["Linux", "tools"]
comments = true
date = "2010-07-14T11:36:11+08:00"
published = true
status = "publish"
tags = ["Linux"]
title = "mencoder提取音频"
type = "post"
description = ""
+++


```sh 
# $1 -- input file
# $2 -- output file
mencoder -oac mp3lame -ovc copy -of rawaudio $1 -o $2
```
<!--more-->