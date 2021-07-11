+++
categories = ["emacs", "org2blog"]
comments = true
published = true
status = "publish"
date = "2011-11-02T11:11:11+08:00"
tags = ["emacs", "org2blog"]
title = "Micolog RSS输出代码背景色问题"
type = "post"
description = ""
+++


大家可能注意到用阅读器读取rss的时候代码看不清（没有背景色的缘故)，所以修改了一下代码的输出：

添加两个函数来插入代码：

```lisp
(defun i-babel-quote (beg end str1 str2)
  (goto-char end)
  (forward-line 1)
  (insert str2)
  (newline)
  (goto-char beg)
  (forward-line -1)
  (newline)
  (insert str1)
)

(defun isrc (St Ed)
  "Enclose code for Emacser.cn"
  (interactive "r")
  (let ((beg St) (end Ed))
    (message "%s %s" beg end)
    (i-babel-quote beg end
```
<!--more-->