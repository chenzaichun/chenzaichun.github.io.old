+++
categories = ["programming"]
comments = true
published = true
status = "publish"
date = "2010-05-07T08:21:23+08:00"
tags = ["curl", "Windows"]
title = "libcurl http下载时文件不存在的问题"
type = "post"
description = ""
+++

当用`libcurl`下载http文件不存在的时候，如果使用`curl_easy_perform`得到的结果也是`CURLE_OK` ，此时不能通过直接通过返回值来判断结果。

可以使用

```c
int code = 0;
curl_easy_getinfo(curl, CURLINFO_RESPONSE_CODE, &code);
if (code != 200) {
     // download error
}
```
<!--more-->