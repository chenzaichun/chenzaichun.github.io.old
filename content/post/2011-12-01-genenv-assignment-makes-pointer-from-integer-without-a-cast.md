+++
categories = ["emacs", "org2blog", "programming"]
comments = true
date = "2011-12-01T22:11:11+08:00"
published = true
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "getenv assignment makes pointer from integer without a cast"
type = "post"
description = ""
+++


今天在使用getenv获取环境变量的时候遇到这个问题，代码类似于这样

```c
#include <stdio.h> 

extern "C" {
   char* getenv(const char*);
}

int main(void) {
  char *value;
  value = getenv("HOME");
  printf("%s\n", value);
}
```

编译的时候会出现一个warning

```
test.c: In function `main': 
test.c:9: warning: assignment makes pointer from integer without a cast
```

运行的时候会出现segment fault

原来是缺少stdlib.h的问题（可是我已经申明了原型呀……）

加入stdlib.h解决问题:

```c
#include <stdlib.h>
```
<!--more-->