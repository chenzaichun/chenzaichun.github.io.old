+++
title = "Linux下让进程生成core文件"
date = "2012-11-26T11:22:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["linux"]
description = ""
+++


在linux下让进程强制生成core dump文件：

```sh
gcore <-o filename> pid
```

<!--more-->