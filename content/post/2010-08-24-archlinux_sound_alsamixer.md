+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-08-24T17:17:33+08:00"
tags = ["archlinux", "Linux"]
title = "lxde音量调节"
type = "post"
description = ""
+++

lxde是没有音量调节的，如果需要调整，只能在term中使用aslamixer进行调节，这样很不方便，幸好可以绑定快捷键，修改~/.config/openbox/lxde-rc.xml，添加


```xml
<!-- volume -->
<keybind key="W-Right">
      <action name="Execute">
	<command>amixer -q set Master 3%-</command>
      </action>
</keybind>
<keybind key="W-Left">
      <action name="Execute">
	<command>amixer -q set Master unmute 3%+</command>
      </action>
</keybind>
```

我绑定的是window＋left，right，其他按键类似
<!--more-->