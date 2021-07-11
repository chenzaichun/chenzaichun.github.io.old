+++
categories = ["Linux", "programming", "tools"]
comments = true
date = "2010-05-08T23:11:38+08:00"
published = true
status = "publish"
tags = ["DVCS", "Linux", "scm"]
title = "git svn clone 特定版本"
type = "post"
description = ""
+++


使用git从svn中clone特定版本：

```sh
git svn clone --prefix=svn/ -s -r$revision:HEAD http://some/svn/repo 
```

<!--more-->