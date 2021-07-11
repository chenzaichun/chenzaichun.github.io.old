+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
date = "2011-04-24T19:24:31+08:00"
status = "publish"
tags = ["archlinux", "Linux", "mingw", "Windows"]
title = "archlinux搭建MinGW跨平台编译环境"
type = "post"
description = ""
+++


由于MinGW在Windows上的速度实在不敢恭维，所以决定在archlinux下搭建MinGW的跨平台编译环境。方法有两种。

1. 通过安装community中的mingw32-gcc实现

```sh
# 安装mingw32-gcc就行，其他的如mingw32-base, mingw32-runtime...等会作为依赖安装
sudo pacman -S mingw32-gcc
```

这种方法安装了最基本的编译环境，如果需要其他的库，则需要自行编译或者通过aur安装。

2. 通过MinGW cross compiling environment来实现。<a href="http://mingw-cross-env.nongnu.org/">http://mingw-cross-env.nongnu.org</a>

```sh
#1. 下载
wget https://bitbucket.org/vog/mingw-cross-env/downloads/mingw-cross-env-2.20.tar.gz

#2. 解压
tar -xzvf mingw-cross-env-2.20.tar.gz

#3. 移动到/opt目录（可选）
mv mingw-cross-env-2.20 /opt/mingw

#4. 安装
su -
cd /opt/mingw
make

#5. 其他的包可以参见mingw-cross-env的文档
```
<!--more-->