+++
title = "Python获取操作系统版本信息"
date = "2012-06-01T09:28:00+08:00"
comments = true
categories = ["programming"]
tags = ["python"]
description = ""
+++


直接上代码：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import platform

print platfrom.version()
print platform.system()
```

对于特定平台，还有相对应的函数，比如 =platform.win32_version()=, =platform.mac_version()=. 详情见 =dir(platform)=
<!--more-->