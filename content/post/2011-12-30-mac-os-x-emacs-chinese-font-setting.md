+++
categories = ["emacs", "org2blog"]
comments = true
published = true
status = "publish"
date = "2011-12-30T23:11:23+08:00"
tags = ["emacs", "macos", "org2blog"]
title = "终于搞定Mac OS X上emacs中文的显示"
type = "post"
description = ""
+++


原来一直没有找到怎么设置，昨天从一个日本网站上抄了一段，貌似工作比较良好：

```lisp
(when (eq window-system 'ns)
  (let ((my-font-height 140)
        (my-font (cond
                  (t   "Monaco")  ;; XCode 3.1 
                  (nil "Menlo")   ;; XCode 3.2
                  ))
        (my-font-ja "STHeiti"))

    (setq mac-allow-anti-aliasing t)
    (setq face-font-rescale-alist
          '(("^-apple-hiragino.*" . 1.2)
            (".*osaka-bold.*" . 1.2)
            (".*osaka-medium.*" . 1.2)
            (".*courier-bold-.*-mac-roman" . 1.0)
            (".*monaco cy-bold-.*-mac-cyrillic" . 0.9)
            (".*monaco-bold-.*-mac-roman" . 0.9)
            ("-cdac$" . 1.3)))

    (when my-font
      (set-face-attribute 'default nil :family my-font :height my-font-height)
      ;;(set-frame-font (format "%s-%d" my-font (/ my-font-height 10)))
      )

    (when my-font-ja
      (let ((fn (frame-parameter nil 'font))
            (rg "iso10646-1"))
        (set-fontset-font fn 'chinese-gb2312 `(,my-font-ja . ,rg))
        (set-fontset-font fn 'chinese-gbk `(,my-font-ja . ,rg))))))
;;        (set-fontset-font fn 'unicode `(,my-font-ja . ,rg))))))
```
<!--more-->