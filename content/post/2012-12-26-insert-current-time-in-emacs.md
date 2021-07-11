+++
title = "emacs中插入当前时间"
date = "2012-11-26T08:49:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["emacs"]
description = ""
+++


emacs中插入当前时间是可以自定义格式的，方法如下：

```lisp
(defun insert-current-time ()
  (interactive)
  (insert (format-time-string "%Y-%m-%d %H:%M:%S" (current-time))))
```
<!--more-->