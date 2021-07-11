+++
categories = ["Linux", "programming", "tools"]
comments = true
date = "2010-05-16T17:11:23+08:00"
published = true
status = "publish"
tags = ["DVCS", "hg", "mercurial"]
title = "mercurial hg代理设置"
type = "post"
description = ""
+++


由于在教育网中,sf上面的代码访问很慢,考虑为mercurial hg增加代理访问. 修改~/.hgrc, 增加


```conf
[http_proxy]
host=127.0.0.1:8580
```

详细的hgrc配置见官方文档: <a href="http://www.selenic.com/mercurial/hgrc.5.html" target="_blank">http://www.selenic.com/mercurial/hgrc.5.html</a>
<!--more-->