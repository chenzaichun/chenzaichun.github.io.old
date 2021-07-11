+++
categories = ["emacs", "programming", "tools"]
comments = true
published = true
date = "2010-06-22T21:12:33+08:00"
status = "publish"
tags = ["emacs", "python"]
title = "emacs下配置python开发环境"
type = "post"
description = ""
+++


1. 安装Pymacs， <a href="http://pymacs.progiciels-bpi.ca/pymacs.html" target="_blank">http://pymacs.progiciels-bpi.ca/pymacs.html</a>

```sh
sudo python setup.py install
```

2. 安装ropemode，<a href="http://pypi.python.org/pypi/ropemode" target="_blank"> http://pypi.python.org/pypi/ropemode</a>

```sh
sudo python setup.py install
```

3. 安装ropemacs，<a href="http://rope.sourceforge.net/ropemacs.html" target="_blank">http://rope.sourceforge.net/ropemacs.html</a>

```sh
sudo python setup.py install
```

4. 在.emacs中加入

```lisp
(autoload 'pymacs-apply "pymacs")(autoload 'pymacs-call "pymacs")(autoload 'pymacs-eval "pymacs" nil t)(autoload 'pymacs-exec "pymacs" nil t)(autoload 'pymacs-load "pymacs" nil t)

(pymacs-load "ropemacs" "rope-")(setq ropemacs-enable-autoimport t)
```
<!--more-->