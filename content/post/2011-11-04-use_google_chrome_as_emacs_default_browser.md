+++
categories = ["emacs", "Linux", "org2blog", "tools"]
comments = true
published = true
status = "publish"
date = "2011-11-04T11:11:11+08:00"
tags = ["emacs", "Google", "Linux", "org2blog"]
title = "将google chrome设置为emacs默认浏览器"
type = "post"
description = ""
+++


```lisp
(setq browse-url-generic-program (executable-find "google-chrome")
      browse-url-browser-function 'browse-url-generic)

```
     
<!--more-->