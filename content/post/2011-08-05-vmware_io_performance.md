+++
categories = ["无所事事"]
comments = true
published = true
status = "publish"
date = "2011-08-15T20:56:31+08:00"
tags = ["无所事事"]
title = "增加vmware虚拟机的性能"
type = "post"
description = ""
+++


vmware跑mac确实卡呀。网上抄了一段，目前还在体验中，效果具体怎么样，还有待观察。

```conf
MemTrimRate=0
sched.mem.pshare.enable = "FALSE"
prefvmx.useRecommendedLockedMemSize = "TRUE"
prefvmx.minVmMemPct = "100"
mainMem.useNamedFile = "FALSE"
MemAllowAutoScaleDown = "FALSE"
mks.enable3d = "FALSE"
mks.noBeep = "true"
logging = "FALSE"
hv.enableIfUnlocked = "TRUE"
```
<!--more-->