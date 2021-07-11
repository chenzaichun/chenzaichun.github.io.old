+++
title = "Certificate not Installed on the Node"
date = "2012-06-28T13:21:00+08:00"
comments = true
categories = ["programming"]
tags = ["tools", "ovo"]
description = ""
+++


今天在启动ovo的时候遇到了这个问题：

> 0: WRN: Thu Jun 28 13:08:42 2012: ovbbccb (1821/4): (bbc-90) The incoming HTTPS client connection from host 127.0.0.1 failed due to the SSL error:
> 1: WRN: Thu Jun 28 13:08:42 2012: ovbbccb (1821/4): (sec.core-116) An SSL connection IO error has occurred. This may be due to a network problem or an SSL handshake er
> ror. Possible causes for SSL handshake errors are that no certificate is installed, an invalid certificate is installed, or the peer does not trust the initiator's cer
> tificate.
> 0: WRN: Thu Jun 28 13:08:43 2012: ovbbccb (1821/9): (bbc-90) The incoming HTTPS client connection from host 127.0.0.1 failed due to the SSL error:
> 1: WRN: Thu Jun 28 13:08:43 2012: ovbbccb (1821/9): (sec.core-116) An SSL connection IO error has occurred. This may be due to a network problem or an SSL handshake er
> ror. Possible causes for SSL handshake errors are that no certificate is installed, an invalid certificate is installed, or the peer does not trust the initiator's cer
> tificate.
> 0: WRN: Thu Jun 28 13:08:44 2012: ovbbccb (1821/5): (bbc-90) The incoming HTTPS client connection from host 127.0.0.1 failed due to the SSL error:
> 1: WRN: Thu Jun 28 13:08:44 2012: ovbbccb (1821/5): (sec.core-116) An SSL connection IO error has occurred. This may be due to a network problem or an SSL handshake er
> ror. Possible causes for SSL handshake errors are that no certificate is installed, an invalid certificate is installed, or the peer does not trust the initiator's cer
> tificate.
> 0: WRN: Thu Jun 28 13:08:44 2012: ovbbccb (1821/6): (bbc-90) The incoming HTTPS client connection from host 127.0.0.1 failed due to the SSL error:
> 1: WRN: Thu Jun 28 13:08:44 2012: ovbbccb (1821/6): (sec.core-116) An SSL connection IO error has occurred. This may be due to a network problem or an SSL handshake er
> ror. Possible causes for SSL handshake errors are that no certificate is installed, an invalid certificate is installed, or the peer does not trust the initiator's cer
> tificate.

<!--more-->

解决办法：

修改`/var/opt/OV/conf/xpl/config/local_settings.ini`, 将`CERT_INSTALLED=TRUE`删除掉，并重启`ovbbccb`


