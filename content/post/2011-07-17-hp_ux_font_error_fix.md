+++
categories = ["Linux", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2011-07-17T20:56:31+08:00"
tags = ["unix"]
title = "X Server连接到hp-ux字体问题解决办法"
type = "post"
description = ""
+++

在用x server连接到hp-ux的时候，open view启动失败：

``` 
Warning: Missing charsets in String to FontSet conversion
Warning: Cannot convert string "-dt-interface system-medium-r-normal-s*-*-*-*-*-*-*-*-*" to type FontSet
Warning: Missing charsets in String to FontSet conversion
Warning: Unable to load any usable fontset
Warning:
Name: FONTLIST_DEFAULT_TAG_STRING
Class: XmRendition
Conversion failed. Cannot load font.

Warning: Cannot convert string "-dt-interface system-medium-r-normal-s*-*-*-*-*-*-*-*-*" to type FontList
Warning: Cannot convert string "-dt-interface system-medium-r-normal-s*-*-*-*-*-*-*-*-*:" to type FontList
Warning:
Name: FONTLIST_DEFAULT_TAG_STRING
Class: XmRendition
Conversion failed. Cannot load font.

Warning: Cannot convert string "-dt-interface system-medium-r-normal-s*-*-*-*-*-*-*-*-*" to type FontList
Warning: Cannot convert string "-dt-interface system-medium-r-normal-s*-*-*-*-*-*-*-*-*:" to type FontList
Warning: Missing charsets in String to FontSet conversion
Warning: Cannot convert string "-dt-interface user-medium-r-normal-s*-*-*-*-*-*-*-*-*" to type FontSet
Warning: Missing charsets in String to FontSet conversion
Warning: Unable to load any usable fontset
Warning:
Name: FONTLIST_DEFAULT_TAG_STRING
Class: XmRendition
Conversion failed. Cannot load font.

Warning: Cannot convert string "-dt-interface user-medium-r-normal-s*-*-*-*-*-*-*-*-*" to type FontList
Warning: Cannot convert string "-dt-interface user-medium-r-normal-s*-*-*-*-*-*-*-*-*:" to type FontList
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Cannot convert string "-*-helvetica-medium-r-*-140-*" to type FontSet
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Unable to load any usable fontset
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Cannot convert string "-*-helvetica-medium-r-*-120-*" to type FontSet
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Unable to load any usable fontset
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Cannot convert string "-*-helvetica-medium-r-*-100-*" to type FontSet
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Unable to load any usable fontset
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Cannot convert string "-*-helvetica-medium-r-*-80-*" to type FontSet
ovw: Xt Warning: Missing charsets in String to FontSet conversion
ovw: Xt Warning: Unable to load any usable fontset
/opt/OV/bin
```

解决办法：
<!--more-->


1. 首先在hp-ux上检查xfs是否启动

```sh
ps -ef |grep xfs
```

2. 如果没有启动，则启动xfs

```sh
# /sbin/init.d/xfs start
```

3. 在需要连接的x server上设置：

```sh
$ xset +fp tcp/hostname:7000
$ xset fp rehash
$ xset -q
$ xhost +
```

4. 然后ssh连上之后，open view显示正常。
