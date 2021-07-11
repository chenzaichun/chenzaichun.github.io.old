+++
categories = ["programming", "tools"]
comments = true
published = true
status = "publish"
date = "2010-07-22T17:26:22+08:00"
tags = ["VC", "Windows"]
title = "CDHtmlDialog中禁用右键菜单和F5刷新"
type = "post"
description = ""
+++


参考：<a href="http://www.codeproject.com/KB/MFC/dhtmldialog.aspx?msg=2016381#xx2016381xx" target="_blank">猛击这里</a>
在头文件中加入：

```cpp
STDMETHOD(ShowContextMenu)(DWORD dwID, POINT *ppt, IUnknown *pcmdtReserved, IDispatch *pdispReserved);
STDMETHOD(TranslateAccelerator)(LPMSG lpMsg, const GUID * pguidCmdGroup, DWORD nCmdID);
```

在cpp中加入：

```cpp
STDMETHODIMP ChtmldialogDlg::ShowContextMenu(DWORD dwID, POINT *ppt, IUnknown *pcmdtReserved, IDispatch *pdispReserved)
{
//return CDHtmlDialog::ShowContextMenu(dwID, ppt, pcmdtReserved, pdispReserved);
return S_OK;
}

STDMETHODIMP ChtmldialogDlg::TranslateAccelerator(LPMSG lpMsg, const GUID * pguidCmdGroup, DWORD nCmdID)
{
if (lpMsg && lpMsg->message == WM_KEYDOWN && lpMsg->wParam == VK_F5) {
return S_OK;
}
return CDHtmlDialog::TranslateAccelerator(lpMsg, pguidCmdGroup, nCmdID);
}
```
<!--more-->