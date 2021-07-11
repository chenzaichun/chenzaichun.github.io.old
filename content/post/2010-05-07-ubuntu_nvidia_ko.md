+++
categories = ["Linux"]
comments = true
published = true
status = "publish"
date = "2010-05-07T16:14:11+08:00"
tags = ["Linux", "nvidia", "ubuntu"]
title = "ubuntu 10.04 unable to load the kernel module 'nvidia.ko'"
type = "post"
description = ""
+++


``` 
ERROR: Unable to load the kernel module 'nvidia.ko'. This happens most   frequently when this kernel module was built against the wrong or   improperly configured kernel sources, with a version of gcc that differs  from the one used to build the target kernel, or if a driver such as   rivafb/nvidiafb is present and prevents the NVIDIA kernel module from   obtaining ownership of the NVIDIA graphics device(s), or NVIDIA GPU   installed in this system is not supported by this NVIDIA Linux graphics   driver release.

Please see the log entries 'Kernel module load  error' and 'Kernel  messages' at the end of the file  '/var/log/nvidia-installer.log' for  more information.

```
解决办法：

```sh
sudo sh NVIDA-Linux-x86_64-190.53-pkg.run -k $(uname -r)
```
<!--more-->