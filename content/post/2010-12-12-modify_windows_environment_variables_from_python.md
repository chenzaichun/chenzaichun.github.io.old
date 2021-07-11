+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-12T18:18:29+08:00"
status = "publish"
tags = ["python"]
title = "python修改windows环境变量"
type = "post"
description = ""
+++

总结一下有2种方法：

1. 通过注册表操作HKLMSYSTEMCurrentControlSetControlSession ManagerEnvironment以达到目的，参考<a href="/python_access_windows_registry.html" target="_blank">_winreg修改注册表</a>，如果要通知其他程序环境变量已改变，则可以使用pywin32中的win32gui模块来实现：

<!--more-->


```python
from _winreg import *
import os, sys, win32gui, win32con

def queryValue(key, name):       
    value, type_id = QueryValueEx(key, name)
    return value

def show(key):
    for i in range(1024):                                           
        try:
            n,v,t = EnumValue(key, i)
            print '%s=%s' % (n, v)
        except EnvironmentError:
            break          

def main():
    try:
        path = r'SYSTEMCurrentControlSetControlSession ManagerEnvironment'
        reg = ConnectRegistry(None, HKEY_LOCAL_MACHINE)
        key = OpenKey(reg, path, 0, KEY_ALL_ACCESS)
        
        if len(sys.argv) == 1:
            show(key)
        else:
            name, value = sys.argv[1].split('=')
            if name.upper() == 'PATH':
                value = queryValue(key, name) + ';' + value
            if value:
                SetValueEx(key, name, 0, REG_EXPAND_SZ, value)
            else:
                DeleteValue(key, name)
            
        win32gui.SendMessage(win32con.HWND_BROADCAST, win32con.WM_SETTINGCHANGE, 0, 'Environment')
                            
    except Exception, e:
        print e

    CloseKey(key)    
    CloseKey(reg)
    
if __name__=='__main__':    
    usage = 
"""
Usage:

Show all environment vsarisbles - enver
Add/Modify/Delete environment variable - enver <name>=[value]

If <name> is PATH enver will append the value prefixed with ;

If there is no value enver will delete the <name> environment variable

Note that the current command window will not be affected, 
only new command windows.
"""
    argc = len(sys.argv)
    if argc > 2 or (argc == 2 and sys.argv[1].find('=') == -1):
        print usage
        sys.exit()
        
    main()    
```

2. 如果系统有setx，可以用os.system("setx xxxxx xxxxxx")这种方式

P.S. 用os.putenv和pywin32中的win32api.SetEnvironmentVariable是没有办法持久化修改环境变量的。修改影响的就是该进程和子进程（相当于shell的export）
