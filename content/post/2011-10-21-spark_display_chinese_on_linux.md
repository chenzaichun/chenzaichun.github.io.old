+++
categories = ["emacs", "Linux", "org2blog"]
comments = true
published = true
status = "publish"
date = "2011-10-21T21:56:31+08:00"
tags = ["emacs", "libjinglea", "Linux", "org2blog", "xmpp"]
title = "Spark在Linux下的乱码解决"
type = "post"
description = ""
+++


@ <a href="http://www.cnblogs.com/highriver/archive/2010/06/24/1764520.html">http://www.cnblogs.com/highriver/archive/2010/06/24/1764520.html</a>  

解决Spark乱码：   

Linux下Java图形界面中文显示乱码问题往往是所合适的jre/fonts里面没有合适的字体。   

比如spark会往~/.install4j里面写入最后一行，说明spark使用的jre是其自带的jre.    

``` 
JRE_VERSION    /home/gaoyibo/work/jdk1.6.0_05    1    6    0    05
JRE_VERSION    /usr    1    6    0    0
JRE_VERSION    /home/gaoyibo/comodo/openfire/Spark/jre    1    6    0    02
```

所以遇到乱码问题解决：   

1，找到jre路径，创建fallback文件夹    

```sh
cd <path_to_spark>/Spark/jre/lib/fonts
sudo mkdir fallback
```
     
进入fallback文件夹，链接中文字体（我选的是文泉驿正黑）    

```sh
cd fallback
sudo ln -s /usr/share/fonts/truetype/wqy/wqy-zenhei.ttc
sudo mkfontdir
sudo mkfontscale
```
       
<!--more-->