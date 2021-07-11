+++
categories = ["emacs", "Linux", "org2blog", "programming", "tools"]
comments = true
date = "2011-12-12T11:11:33+08:00"
published = true
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "archlinux下配置GNUStep环境"
type = "post"
description = ""
+++


GNUStep 主要有一下四部分组成，对应着在 Archlinux 系统下面我们也需要安装四个软件包。

1. GNUstep Make: 提供类似 Makefile 的功能, 称为 GNUmakefile, 较 Makefile 好用许多。

2. GNUstep Base: 提供 OpenStep 的 Foundation 程式库, 处理非图形介面的功能。

3. GNUstep GUI: 提供 OpenStep 的 AppKit 程式库, 处理图形介面的功能。

4. GNUstep Back: 提供与作业系统相关的后端处理, 提供 GNUstep GUI 有关绘图及字型的功能。 

安装GNUStep：

```sh
sudo pacman -S gnustep-base gnustep-make gnustep-gui gnustep-back
```

当然也需要安装gcc objc支持:

```sh
sudo pacman -S gcc-objc
```

编译obj-c所需命令:

```sh
gcc `gnustep-config --objc-flags` -lgnustep-base -lobjc xxx.m -o xxx
```

Hello World测试:

```objc
//helloworld.m
#import "Foundation/Foundation.h"

int main(int argc, const char *argv[])
{
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    NSLog(@"Hello World\n");
    [pool drain];
    return 0;
}
```

编译运行：

```sh
gcc `gnustep-config --objc-flags` -lgnustep-base -lobjc helloworld.m -o helloworld
```

如果没有添加-lobjc的话，新版的gcc编辑器会出现链接错误：

> /usr/bin/ld: /usr/lib/gcc/i686-pc-linux-gnu/4.6.2/../../../libgnustep-base.so: undefined reference to symbol 'objc_get_class'
> /usr/bin/ld: note: 'objc_get_class' is defined in DSO /usr/lib/libobjc.so.3 so try adding it to the linker command line
> /usr/lib/libobjc.so.3: could not read symbols: Invalid operation
> collect2: ld returned 1 exit status

运行结果：
```
$ ./helloworld
2011-12-12 03:27:14.391 helloworld[6928] No local time zone specified.
2011-12-12 03:27:14.392 helloworld[6928] Using time zone with absolute offset 0.
2011-12-12 03:27:14.338 helloworld[6928] Hello World
```

这里有2个warning, 设置一下: 
```sh
defaults write NSGlobalDomain "Local Time Zone" 'Asia/Chongqing'
```
      
<!--more-->