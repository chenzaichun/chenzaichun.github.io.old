+++
categories = ["emacs", "Linux"]
comments = true
published = true
date = "2010-05-07T18:19:20+08:00"
status = "publish"
tags = ["emacs", "Linux"]
title = "Linux下修改 emacs默认背景颜色"
type = "post"
description = ""
+++


创建`~/.Xresources`，添加：
``` conf
Xft.antialias: 1
Xft.hinting: 1
Xft.hintstyle: hintfull
Emacs.FontBackend: xft
Emacs.Font: WenQuanYi Zen Hei Mono-16
Emacs.Geometry: 80x25
Emacs*Foreground: white
Emacs*Background: black
```

执行:

```sh
xdb ~/.Xresources
```

重启emacs。下次启动背景色自动就改变了！;-)
<!--more-->