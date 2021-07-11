+++
categories = ["programming", "tools"]
comments = true
published = true
date = "2010-12-13T19:54:31+08:00"
status = "publish"
tags = ["python"]
title = "Python操作Windows Service"
type = "post"
description = ""
+++


python中可以通过pywin32操作Windows Service：

```python
import win32service

def PrintServiceStatus(status):
    svcType, svcState, svcControls, err, svcErr, svcCP, svcWH = status
    if svcType & win32service.SERVICE_WIN32_OWN_PROCESS:
        print "The service runs in its own process"
    if svcType & win32service.SERVICE_WIN32_SHARE_PROCESS:
        print "The service shares a process with other services"
    if svcType & win32service.SERVICE_INTERACTIVE_PROCESS:
        print "The service can interact with the desktop"
    # Other svcType flags not shown.
    if svcState==win32service.SERVICE_STOPPED:
        print "The service is stopped"
    elif svcState==win32service.SERVICE_START_PENDING:
        print "The service is starting"
    elif svcState==win32service.SERVICE_STOP_PENDING:
        print "The service is stopping"
    elif svcState==win32service.SERVICE_RUNNING:
        print "The service is running"
    # Other svcState flags not shown.
    if svcControls & win32service.SERVICE_ACCEPT_STOP:
        print "The service can be stopped"
    if svcControls & win32service.SERVICE_ACCEPT_PAUSE_CONTINUE:
        print "The service can be paused"
    # Other svcControls flags not shown

if __name__ == '__main__':
    scm = win32service.OpenSCManager(None, None,
                                     win32service.SC_MANAGER_ALL_ACCESS)
    allservice = win32service.EnumServicesStatus(scm, win32service.SERVICE_WIN32, win32service.SERVICE_STATE_ALL)
    svc = win32service.OpenService(scm, 'Cash Download',
                                   win32service.SC_MANAGER_ALL_ACCESS)
    win32service.ChangeServiceConfig(svc, win32service.SERVICE_NO_CHANGE,
                                     win32service.SERVICE_AUTO_START,
                                     win32service.SERVICE_NO_CHANGE,
                                     None, None, 0, None, None, None, None)
    win32service.StartService(svc, "")
    win32service.ControlService(svc, win32service.SERVICE_CONTROL_STOP)
    win32service.ControlService(svc, win32service.SERVICE_CONTROL_START)
    PrintServiceStatus(win32service.QueryServiceStatus(svc))
```
<!--more-->