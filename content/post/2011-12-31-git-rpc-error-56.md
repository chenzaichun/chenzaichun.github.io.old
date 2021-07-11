+++
categories = ["emacs", "org2blog", "programming", "tools"]
comments = true
published = true
status = "publish"
date = "2011-12-31T19:23:56+08:00"
tags = ["DVCS", "emacs", "git", "org2blog", "VCS"]
title = "git rpc error 56"
type = "post"
description = ""
+++


今天使用git push origin master的时候出现了这个问题： 

> Counting objects: 592, done.
> Delta compression using up to 2 threads.
> Compressing objects: 100% (584/584), done.
> error: RPC failed; result=56, HTTP code = 0
> fatal: The remote end hung up unexpectedly
> Writing objects: 100% (592/592), 12.77 MiB | 10.10 MiB/s, done.
> Total 592 (delta 159), reused 0 (delta 0)
> fatal: The remote end hung up unexpectedly
> fatal: expected ok/error, helper said '4004?Va???6?)&?'?~$latex ?pt?-i?BS??Κ?hSK}??lmZri?=kf?v??????AqG?*??*|q??k?+$?\0Q?

解决办法参考[这里](http://codaset.com/codaset/codaset/tickets/723)

失败的原因就是git `HTTP buffer`太小，设置大一点就没有问题，比如：

```sh
git config http.postBuffer 524288000
```
   
<!--more-->