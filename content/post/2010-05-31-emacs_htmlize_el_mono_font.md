+++
categories = ["emacs", "programming", "tools"]
comments = true
date = "2010-05-31T22:43:11+08:00"
published = true
status = "publish"
tags = ["emacs"]
title = "设置emacs htmlize.el为等宽字体"
type = "post"
description = ""
+++


我用emacs的htmlize.el elisp来给代码上色，默认情况下并没有设置成为等宽字体，所以显示上不太好看，为htmlize默认添加输出为等宽字体：

```lisp
(defun htmlize-font-pre-tag (face-map)
  (let ((fstruct (gethash 'default face-map)))
    (format "<pre style="font-family: 'courier new', courier;color:%s;background-color:%s">"
            (htmlize-fstruct-foreground fstruct)
            (htmlize-fstruct-background fstruct))))
```
<!--more-->