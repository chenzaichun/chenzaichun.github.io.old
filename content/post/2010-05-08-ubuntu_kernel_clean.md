+++
categories = ["Linux", "tools"]
comments = true
date = "2010-05-08T19:33:11+08:00"
published = true
status = "publish"
tags = ["kernel", "Linux", "ubuntu"]
title = "ubuntu删除无用旧内核"
type = "post"
description = ""
+++

用这个可以删除旧内核

```sh
sudo aptitude purge ~ilinux-image-.*(!`uname -r`)
```

<a class="postlink" href="http://forum.ubuntu.org.cn/viewtopic.php?t=129876">http://forum.ubuntu.org.cn/viewtopic.php?t=129876</a>
<!--more-->