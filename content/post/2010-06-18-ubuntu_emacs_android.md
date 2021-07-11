+++
categories = ["Linux", "programming", "tools"]
comments = true
date = "2010-06-18T12:32:33+08:00"
published = true
status = "publish"
tags = ["android", "emacs", "Linux"]
title = "ubuntu下配置emacs android开发环境"
type = "post"
description = ""
+++


1. 首先安装jdk

```sh
sudo apt-get install openjdk-6-jdk
```

2. 下载并解压android sdk

3. 将adnroid sdk目录下的tools添加到PATH

4. 安装android-mode

```sh
git clone git://github.com/remvee/android-mode.git
```

5. 创建一个android虚拟设备如test1，在.emacs中配置android-mode

```lisp
;; android-mode
(add-to-list 'load-path "~/android/android-mode")
(require 'android-mode)
(custom-set-variables
 '(android-mode-avd "test1")
 '(android-mode-sdk-dir "~/android/"))
```


6. 测试模拟器是否正常

``` 
android-start-emulator
```

7. 安装ant

```sh
sudo apt-get instal ant
```

8. 中创建一个android工程使用命令android create project

```sh
android create project -n helloworld -t 12 -p ./ -k org.example.helloworld -a helloworld
```

9. 在emacs安装工程

``` 
M-x android-ant-install
```

10. 重新安装工程使用

``` 
M-x android-ant-reinstall
```
<!--more-->