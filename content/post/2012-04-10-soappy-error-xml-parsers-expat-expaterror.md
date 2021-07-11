+++
categories = ["emacs", "org2blog", "programming"]
comments = true
published = true
status = "publish"
date = "2012-04-10T20:13:24+08:00"
tags = ["emacs", "org2blog", "python", "web service"]
title = "SOAPpy Error"
type = "post"
description = ""
+++

今天准备用SOAPpy来写一个最简单的soap client，代码如下：

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

from SOAPpy import WSDL
WSDLfile = "/path/to/webservice"

wsdlObject = WSDL.Proxy(WSDLfile)

print 'Available methods:'
for method in wsdlObject.methods.keys() :
    print method
    ci = wsdlObject.methods[method]
    # you can also use ci.inparams
    for param in ci.outparams :
        # list of the function and type 
        # depending of the wsdl...
        print param.name.ljust(20) , param.type
    print
```

运行的时候出现了这个问题：

<!--more-->

> Traceback (most recent call last):
>   File "remedy_ws_client.py", line 9, in <module>
>     wsdlObject = WSDL.Proxy(WSDLfile)
>   File "build/bdist.macosx-10.6-universal/egg/SOAPpy/WSDL.py", line 85, in __init__
>   File "/Library/Python/2.6/site-packages/wstools-0.3-py2.6.egg/wstools/WSDLTools.py", line 47, in loadFromString
>     return self.loadFromStream(StringIO(data))
>   File "/Library/Python/2.6/site-packages/wstools-0.3-py2.6.egg/wstools/WSDLTools.py", line 28, in loadFromStream
>     document = DOM.loadDocument(stream)
>   File "/Library/Python/2.6/site-packages/wstools-0.3-py2.6.egg/wstools/Utility.py", line 635, in loadDocument
>     return xml.dom.minidom.parse(data)
>   File "/System/Library/Frameworks/Python.framework/Versions/2.6/lib/python2.6/xml/dom/minidom.py", line 1918, in parse
>     return expatbuilder.parse(file)
>   File "/System/Library/Frameworks/Python.framework/Versions/2.6/lib/python2.6/xml/dom/expatbuilder.py", line 928, in parse
>     result = builder.parseFile(file)
>   File "/System/Library/Frameworks/Python.framework/Versions/2.6/lib/python2.6/xml/dom/expatbuilder.py", line 207, in parseFile
>     parser.Parse(buffer, 0)
> xml.parsers.expat.ExpatError: not well-formed (invalid token): line 1, column 5


查了很久，最后发现是因为公司网络需要代理的缘故，造成python不能连接网络。解决办法就是: 

```sh
export http_proxy=proxy_server:port
export https_proxy=proxy_server:port
```

