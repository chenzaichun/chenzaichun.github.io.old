+++
categories = ["Linux", "tools"]
comments = true
published = true
date = "2010-06-06T12:37:11+08:00"
status = "publish"
tags = ["ubuntu"]
title = "命令行下查看deb包的依赖"
type = "post"
description = ""
+++

命令行下察看deb包的依赖(如察看google chrome的依赖）：

```sh
dpkg --info google-chrome-unstable_current_amd64.deb | grep Depends
 Pre-Depends: dpkg (>= 1.14.0)
 Depends: ca-certificates, libasound2, libatk1.0-0 (>= 1.13.2), libbz2-1.0, libc6 (>= 2.6-1), libcairo2 (>= 1.4.0), libfontconfig1 (>= 2.4.0), libfreetype6 (>= 2.3.5), libgcc1 (>= 1:4.2.1), libgconf2-4, libglib2.0-0 (>= 2.14.0), libgtk2.0-0 (>= 2.12.0), libjpeg62, libnspr4-0d (>= 4.7.1), libnss3-1d (>= 3.12.3), libpango1.0-0 (>= 1.18.3), libpng12-0, libstdc++6 (>= 4.2.1), libxslt1.1, libxss1, lsb-base (>= 3.2), wget, xdg-utils (>= 1.0.1), zlib1g (>= 1:1.2.3.3.dfsg-1)
```
 
<!--more-->