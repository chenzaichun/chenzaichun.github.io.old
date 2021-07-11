+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-06-21T11:23:31+08:00"
tags = ["python", "telnet"]
title = "python telnetlib第一例"
type = "post"
description = ""
+++


参考limodou

```python
#!/usr/bin/env python
#-*- coding: utf-8 -*-
import telnetlib

host = {}
host['ip']='127.0.0.1'
host['user']='user'
host['password']='password***'
host['commands']=['cd /home/xxxxx', 'ls -l --color=no']

def do(host):    
    tn = telnetlib.Telnet(host['ip'])
#    tn.set_debuglevel(2)    
    tn.read_until("login: ")    
    tn.write(host['user'] + "n")    
    tn.read_until("Password: ")    
    tn.write(host['password'] + "n")

#    tn.read_all()    

    for command in host['commands']:        
    	tn.write(command+'n')

    tn.write("exit")    
    print tn.read_all()

    print 'Finish!'

do(host)
```
<!--more-->