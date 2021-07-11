+++
title = "SUSE Enterprise Linux 10.x下安装JDK/JRE"
date = "2012-10-25T15:06:00+08:00"
comments = true
categories = ["tools", "programming"]
tags = ["linux", "suse"]
description = ""
+++


SUSE Enterprise Linux安装盘中是不包含non-public的安装文件的，
如果是要安装默认的JDK，只能选择ibm的版本

以下是安装1.5版本的jdk的方法

```sh
# 64bits
yast2 -i java-1_5_0-ibm-devel 

# 32bits
yast2 -i java-1_5_0-ibm-devel-32bit
```

如果要安装sun版本的java，则需要通过手动安装的方式：

<!--more-->

1. 下载安装包,[Java download](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

如下载1.6版本的64位版本: http://download.oracle.com/otn-pub/java/jdk/6u37-b06/jdk-6u37-linux-x64-rpm.bin

2. 安装

```sh
# This creates a file named jdk-6unn-linux-version.rpm, 
# and a directory named /usr/java/jdk.1.6.0_nn with 
# the Sun JDK software in it.
sh ./jdk-6u37-linux-x64-rpm.bin
```

安装日志

```
# sh jdk-6u37-linux-x64-rpm.bin 
Unpacking...
Checksumming...
Extracting...
UnZipSFX 5.50 of 17 February 2002, by Info-ZIP (Zip-Bugs@lists.wku.edu).
  inflating: jdk-6u37-linux-amd64.rpm  
  inflating: sun-javadb-common-10.6.2-1.1.i386.rpm  
  inflating: sun-javadb-core-10.6.2-1.1.i386.rpm  
  inflating: sun-javadb-client-10.6.2-1.1.i386.rpm  
  inflating: sun-javadb-demo-10.6.2-1.1.i386.rpm  
  inflating: sun-javadb-docs-10.6.2-1.1.i386.rpm  
  inflating: sun-javadb-javadoc-10.6.2-1.1.i386.rpm  
Preparing...                ########################################### [100%]
   1:jdk                    ########################################### [100%]
Unpacking JAR files...
	rt.jar...
	jsse.jar...
	charsets.jar...
	tools.jar...
	localedata.jar...
	plugin.jar...
	javaws.jar...
	deploy.jar...
warning: waiting to reestablish exclusive database lock
error: %post(jdk-1.6.0_37-fcs.x86_64) scriptlet failed, exit status 5
Installing JavaDB
Preparing...                ########################################### [100%]
   1:sun-javadb-common      ########################################### [ 17%]
   2:sun-javadb-core        ########################################### [ 33%]
   3:sun-javadb-client      ########################################### [ 50%]
   4:sun-javadb-demo        ########################################### [ 67%]
   5:sun-javadb-docs        ########################################### [ 83%]
   6:sun-javadb-javadoc     ########################################### [100%]

Java(TM) SE Development Kit 6 successfully installed.

Product Registration is FREE and includes many benefits:
* Notification of new versions, patches, and updates
* Special offers on Oracle products, services and training
* Access to early releases and documentation

Product and system data will be collected. If your configuration
supports a browser, the JDK Product Registration form will
be presented. If you do not register, none of this information
will be saved. You may also register your JDK later by
opening the register.html file (located in the JDK installation
directory) in a browser.

For more information on what data Registration collects and 
how it is managed and used, see:
http://java.sun.com/javase/registration/JDKRegistrationPrivacy.html

Press Enter to continue.....

 
Done.
```
