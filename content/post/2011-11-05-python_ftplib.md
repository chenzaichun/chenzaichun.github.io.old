+++
categories = ["emacs", "org2blog", "programming"]
comments = true
published = true
status = "publish"
tags = ["emacs", "org2blog", "python"]
title = "Python ftplib"
date = "2011-11-05T11:11:11+08:00"
type = "post"
description = ""
+++


```python
#!/usr/bin/env python
import ftplib
session = ftplib.FTP('server','usr','pwd')
myfile = open('test.txt','rb')
session.storbinary('STOR test.txt', myfile)
myfile.close()
session.quit()
     
```
<!--more-->