+++
categories = ["emacs", "org2blog"]
comments = true
date = "2011-12-09T11:23:21+08:00"
published = true
status = "publish"
tags = ["emacs", "org2blog"]
title = "emacs代理设置"
type = "post"
description = ""
+++


在Linux上，用环境变量的方式可以直接让emacs使用代理，如设置http_proxy
```sh
export http_proxy=xxx.xx.xx.xx:port
```

但是在mac上这样做就不行了。由于elisp中一般http都是使用的`url package`，所以可以通过`url package`的接口来设置代理:

```lisp
(setq url-using-proxy t)
(setq url-proxy-services '(("http" . "proxyserver:port")))
```
<!--more-->