+++
categories = ["Linux", "programming", "tools"]
comments = true
date = "2010-07-05T12:32:33+08:00"
published = true
status = "publish"
tags = ["eclipse"]
title = "eclipse老是自动退出"
type = "post"
description = ""
+++


今天在使用eclipse的时候，一会儿就自动退出，重试很多次无果！前两天都还是好好的，怎么会这样呢？后来一想，昨天升级ubuntu的时候升级了openjdk，难道是这个原因？删除openjdk，然后安装sun-java6-jdk。ubuntu下在/etc/apt/sources.list添加：

```conf
deb http://archive.canonical.com/ lucid partner 
```

测试之后，搞定！如果遇到同样问题，多半是jdk的问题。
<!--more-->