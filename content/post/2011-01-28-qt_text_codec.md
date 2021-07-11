+++
categories = ["programming"]
comments = true
published = true
date = "2011-01-28T19:34:31+08:00"
status = "publish"
tags = ["Qt"]
title = "QT中文乱码问题"
type = "post"
description = ""
+++


参考链接：<a href="http://hi.baidu.com/ilinux/blog/item/3603b801e8ce5907738da581.html">http://hi.baidu.com/ilinux/blog/item/3603b801e8ce5907738da581.html</a>

因为我emacs的默认编码设置的是utf-8，所以这种方法是最方便的。

```cpp
#include <QTextCodec>

int main(int argc, char *argv[])
{
    QTextCodec::setCodecForTr(QTextCodec::codecForName("UTF8"));
    QTextCodec::setCodecForLocale(QTextCodec::codecForName("UTF8"));
    QTextCodec::setCodecForCStrings(QTextCodec::codecForName("UTF8"));

...
}
```
<!--more-->