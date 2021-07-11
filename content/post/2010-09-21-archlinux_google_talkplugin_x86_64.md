+++
categories = ["Linux", "tools"]
comments = true
published = true
status = "publish"
date = "2010-09-21T18:17:15+08:00"
tags = ["archlinux", "Google", "Linux"]
title = "archlinux x86_64安装google-talkplugin"
type = "post"
description = ""
+++


要想在gmail里面使用视频，音频或者打电话，需要安装google-talkplugin，官方网站提供了deb和rpm包，archlinux下安装可以通过aur安装

```sh
yaourt -S google-talkplugin
```

这个时候直接不能安装，因为安装过程会提示lib32-openssl-compatible找不到，修改PKGBUILD，改为lib32-openssl
然后安装之后，会发现启动不了

``` 
[226:202] Waiting for GoogleTalkPlugin to start...
[227:297] Warning(clientchannel.cc:583): Unreadable or no port file.  Could not initiate GoogleTalkPlugin connection
[227:298] Warning(clientchannel.cc:439): Could not initiate GoogleTalkPlugin connection
```

执行/opt/google/talkplugin/GoogleTalkPlugin会提示找不到libssl.so.0.9.8

``` 
./GoogleTalkPlugin: error while loading shared libraries: libssl.so.0.9.8: cannot open shared object file: No such file or directory
```

创建一个软链接

```sh
sudo ln -s /usr/lib32/libssl.so.1.0.0 /usr/lib32/libssl.so.0.9.8
```

还缺少一个东东

``` 
./GoogleTalkPlugin: error while loading shared libraries: libcrypto.so.0.9.8: cannot open shared object file: No such file or directory
```

再创建一个软链接

```sh
sudo ln -s /usr/lib32/libcrypto.so.1.0.0 /usr/lib32/libcrypto.so.0.9.8
```

（注意，file GoogleTalkPlugin竟然是一个32位的elf）

```sh
[chenzaichun@czc-laptop talkplugin]$ file GoogleTalkPlugin
GoogleTalkPlugin: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), stripped
```

所以链接目录应该在/usr/lib32，而不是/usr/lib

完毕，可以正常使用了。
<!--more-->