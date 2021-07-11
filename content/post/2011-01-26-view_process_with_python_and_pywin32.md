+++
categories = ["programming", "tools"]
comments = true
date = "2011-01-26T19:34:31+08:00"
published = true
status = "publish"
tags = ["python", "Windows"]
title = "在Windows下用Python查看进程信息"
type = "post"
description = ""
+++


得益于PyWin32的强大，在Windows下可以通过Python调用Performon COM接口来查看进程的信息。下面的代码就是查看svhost进程的相关信息的示例代码，如果需要其他信息，请自行添加counter

```python
#/!/usr/bin/env python
# -*- coding: utf-8 -*-

import win32api, win32pdh, win32pdhutil
import time

def ShowAllProcesses():

    procs = []
    object = win32pdhutil.find_pdh_counter_localized_name("Process")

    items, instances = win32pdh.EnumObjectItems(None,None,object,win32pdh.PERF_DETAIL_WIZARD)
    instance_dict = {}

    for instance in instances:
        try:
            if instance == 'svchost':
                instance_dict[instance] = instance_dict[instance] + 1
        except KeyError:
            instance_dict[instance] = 0

    items = [win32pdhutil.find_pdh_counter_localized_name("ID Process")] + items[0:]

    for instance, max_instances in instance_dict.items():

        for inum in xrange(max_instances+1):

            hq = win32pdh.OpenQuery()

            hcs = []
            for item in items:
                path = win32pdh.MakeCounterPath((None,object, instance,None, inum, item))
                hcs.append(win32pdh.AddCounter(hq, path))

            win32pdh.CollectQueryData(hq)

            time.sleep(0.01)
            win32pdh.CollectQueryData(hq)
            proc = instance[:15]
            hc = hcs[0]

            vals = []
            for i in range(len(hcs)):
                hc = hcs[i]
                type, val = win32pdh.GetFormattedCounterValue(hc, win32pdh.PDH_FMT_LONG)
                vals.append(val)

            win32pdh.RemoveCounter(hc)

            procs.append([proc, [vals] ])
            win32pdh.CloseQuery(hq)

    print procs
    return procs
ShowAllProcesses()
```
<!--more-->