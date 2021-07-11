+++
categories = ["emacs", "org2blog", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2012-02-06T18:32:34+08:00"
tags = ["emacs", "macos", "org2blog"]
title = "MacOS X emacs拖拽打开文件"
type = "post"
description = ""
+++

emacs在mac os x中拖拽文件到buffer会在buffer中嵌入显示拖拽文件的内容，解决办法：

在`.emacs`中加入

```lisp
(define-key global-map [ns-drag-file] 'my-ns-open-files)
(defun my-ns-open-files ()
  "Open files in the list `ns-input-file'."
  (interactive)
  (mapc 'find-file ns-input-file)
  (setq ns-input-file nil))
```
   
<!--more-->