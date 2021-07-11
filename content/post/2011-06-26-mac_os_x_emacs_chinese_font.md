+++
categories = ["tools"]
comments = true
published = true
status = "publish"
date = "2011-06-26T19:44:31+08:00"
tags = ["emacs"]
title = "emacs中文显示方框"
type = "post"
description = ""
+++


emacs从23版本开始，对于unicode支持就很好了，在windows和linux下对于中文字体的设置也比较方便：

```lisp
(set-default-font "WenQuanYi Zen Hei Mono-12")
(set-fontset-font "fontset-default"
                  'unicode '("WenQuanYi Zen Hei Mono-12" . "unicode-bmp"))
(setq default-frame-alist
      (append '((font . "WenQuanYi Zen Hei Mono-12")) default-frame-alist))

```

可是在mac os X下，这样设置出来的emacs，中文显示为方框。google了一下，没有找到解决办法 :(

```lisp
(x-list-fonts "gb") # ctrl+j
nil
```

gb编码的所有字符在字体中找不到。求高手解答.
<!--more-->