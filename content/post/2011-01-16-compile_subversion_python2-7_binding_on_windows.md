+++
categories = ["programming", "tools"]
comments = true
date = "2011-01-16T19:14:31+08:00"
published = true
status = "publish"
tags = ["python", "svn"]
title = "windows下编译subversion的Python绑定（附下载）"
type = "post"
description = ""
+++


由于svn的python绑定一直没有推出python2.7的版本，所以只好自己编译了。参考链接：<a href="http://www.lejordet.com/2009/03/compiling-subversion-python-bindings-on-windows/">http://www.lejordet.com/2009/03/compiling-subversion-python-bindings-on-windows/</a>
1. 首先下载swig的windows bin，下载地址：<a href="http://www.swig.org/download.html">http://www.swig.org/download.html</a>，并解压到目录A
2. 下载subversion源代码：<a href="http://subversion.tigris.org/servlets/ProjectDocumentList?folderID=10339&expandFolder=10339&folderID=260">http://subversion.tigris.org/servlets/ProjectDocumentList?folderID=10339&expandFolder=10339&folderID=260</a>，并解压到目录A
3. 在下面的链接中下载对应版本的deps<span style="color: #ff0000;">（windows下要下载zip格式的文件，不然vc的dsp文件会用错误的line ending而无法打开）</span>，并解压到目录A
4. cd到目录A，执行命令生成vc的solution文件<span style="color: #ff0000;">（注意自己对应相应的目录，并使用绝对路径）</span>

```sh
gen-make.py -t vcproj --vsnet-version=2008 --with-swig=c:homesrcsubversionswig --with-zlib=c:homesrcsubversionsubversionzlib --with-apr=c:homesrcsubversionsubversionapr --with-apr-util=c:homesrcsubversionsubversionapr-util --with-apr-iconv=c:homesrcsubversionsubversionapr-iconv
```

5. cd到apr目录，用vc打开apr.dsw，并编译工程
6. cd到apr-util目录，编译apr-util.dsw，当询问xxx project已经存在，是否加载的时候，选择yes。有些工程可能编译不过，不用管它
7. 编译根目录下subversion_vcnet.sln，有些工程编译不过，不用管它
8. 创建一个目录B，拷贝subversionbindingsswigpython下svn目录到B
9. 在B目录下创建一个目录libsvn，拷贝bindingsswigpython下的*.py到libsvn
10. 在根目录下搜索*.dll，并拷贝到libsvn目录下
11. 重命名所有已_开头的dll为pyd。
12. 拷贝B目录下的svn和libsvn目录到<PYTHON>Libsite-packages目录下
13. 测试是否正常工作：在python的cmd下输入

```python
from svn import core
```

14. 如果有错误发生，一般是缺少dll的问题，再次确认所有dll都拷贝到libsvn目录下。
这里附上我的编译结果（解压到<Python>Libsite-packages目录下即可使用）：
<a href="http://commondatastorage.googleapis.com/czc_public/appspotmedia/svn-python-1.6.13.win32-py2.7.7z" target="_blank">svn-python-1.6.13.win32-py2.7.7z</a> 
<a href="http://commondatastorage.googleapis.com/czc_public/appspotmedia/svn-python-1.6.13.win32-py2.7.zip" target="_blank">svn-python-1.6.13.win32-py2.7.zip</a> 
<!--more-->