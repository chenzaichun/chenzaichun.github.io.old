+++
categories = ["emacs", "Linux", "org2blog", "programming", "tools"]
comments = true
date = "2011-12-12T09:11:31+08:00"
published = true
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "第一个GNUstep窗口程序"
type = "post"
description = ""
+++


[参考链接](http://gnustep.made-it.com/GSPT/xml/Tutorial_en.html#AEN365)

main.m

```objc
#include "AppController.h"
#include <AppKit/AppKit.h>

int main(int argc, const char *argv[])
{
   NSAutoreleasePool *pool;
   AppController *delegate;

   pool = [[NSAutoreleasePool alloc] init];
   delegate = [[AppController alloc] init];

   [NSApplication sharedApplication];
   [NSApp setDelegate: delegate];

   RELEASE(pool);
   return NSApplicationMain (argc, argv);
}
```

AppController.h

<!--more-->

```objc
#ifndef _AppController_H_
#define _AppController_H_

#include <Foundation/NSObject.h>

@class NSWindow;
@class NSTextField;
@class NSNotification;

@interface AppController : NSObject
{
   NSWindow *window;
   NSTextField *label;
}

- (void)applicationWillFinishLaunching: (NSNotification *) not;
- (void)applicationDidFinishLaunching: (NSNotification *) not;

@end

#endif /* _AppController_H_ */
```

AppController.m

```objc
#include "AppController.h"
#include <AppKit/AppKit.h>

@implementation AppController
- (void) applicationWillFinishLaunching: (NSNotification *) not
{
   /* Create Menu */
   NSMenu *menu;
   NSMenu *info;

   menu = [NSMenu new];
   [menu addItemWithTitle: @"Info"
                   action: NULL
            keyEquivalent: @""];
   [menu addItemWithTitle: @"Hide"
                   action: @selector(hide:)
            keyEquivalent: @"h"];
   [menu addItemWithTitle: @"Quit"
                   action: @selector(terminate:)
            keyEquivalent: @"q"];

   info = [NSMenu new];
   [info addItemWithTitle: @"Info Panel..."
                   action: @selector(orderFrontStandardInfoPanel:)
            keyEquivalent: @""];
   [info addItemWithTitle: @"Preferences"
                   action: NULL
            keyEquivalent: @""];
   [info addItemWithTitle: @"Help"
                   action: @selector (orderFrontHelpPanel:)
            keyEquivalent: @"?"];

   [menu setSubmenu: info
            forItem: [menu itemWithTitle:@"Info"]];
   RELEASE(info);

   [NSApp setMainMenu:menu];
   RELEASE(menu);

   /* Create Window */
   window = [[NSWindow alloc] initWithContentRect: NSMakeRect(300, 300, 200, 100)
                                        styleMask: (NSTitledWindowMask |
                                                    NSMiniaturizableWindowMask |
                                                    NSResizableWindowMask)
                                          backing: NSBackingStoreBuffered
                                            defer: YES];
   [window setTitle: @"Hello World"];

   /* Create Label */
   label = [[NSTextField alloc] initWithFrame: NSMakeRect(30, 30, 80, 30)];
   [label setSelectable: NO];
   [label setBezeled: NO];
   [label setDrawsBackground: NO];
   [label setStringValue: @"Hello World"];

   [[window contentView] addSubview: label];
   RELEASE(label);
}

- (void) applicationDidFinishLaunching: (NSNotification *) not
{
   [window makeKeyAndOrderFront: self];
}

- (void) dealloc
{
  RELEASE(window);
  [super dealloc];
}

@end
```

HelloWorldInfo.plist

```conf
{
   ApplicationDescription = "Hello World Tutorial";
   ApplicationIcon = "";
   ApplicationName = HelloWorld;
   ApplicationRelease = 0.1;
   Authors = "";
   Copyright = "Copyright (C) 200x by ...";
   CopyrightDescription = "Released under...";
   FullVersionID = 0.1;
   URL = "";
}
```

GNUmakefile

```makefile
#GNUSTEP_MAKEFILES=/usr/share/GNUstep/Makefiles
GNUSTEP_MAKEFILES=$(shell gnustep-config --variable=GNUSTEP_MAKEFILES)
include $(GNUSTEP_MAKEFILES)/common.make

APP_NAME = HelloWorld
HelloWorld_HEADERS = AppController.h
HelloWorld_OBJC_FILES = main.m AppController.m
HelloWorld_RESOURCE_FILES = HelloWorldInfo.plist

include $(GNUSTEP_MAKEFILES)/application.make
```

这里，需要使用gnustep-config –variable=GNUSTEP_MAKEFILES获取makefiles的路径

编译运行：

```sh
make
openapp ./HelloWorld.app
```

截图:

{% img /media/wpid-gnustep-helloworld.png %}


源代码下载: [HelloWorld.zip](/images/uploads/2011/12/helloworld.zip)
