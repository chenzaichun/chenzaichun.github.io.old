+++
title = "cobbler 安装"
date = 2017-11-26
lastmod = 2017-11-28T09:06:38+08:00
tags = ["hugo", "org", "cobbler"]
categories = ["emacs", "linux"]
draft = false
+++

## 环境情况 {#环境情况}

用于PXE推送测试的相关环境信息如下：

|                |         |             |                       |
|----------------|---------|-------------|-----------------------|
| Cobbler Server | Centos7 | 172.16.3.30 | root/Pioneertest@2016 |

Cobbler Server网络可以访问测试用物理服务器IPMI及PXE网卡。物理服务器默认连接一根网线用于PXE推送用途。

| 物理设备名称 | IPMI带外管理地址 | 带外管理用户名 | 带外管理密码 | PXE网卡     |
|--------|------------|---------|--------|-----------|
| 华为2288v3-1 | 172.16.2.20 | root    | Huawei12#$ | 默认从pxe boot |
| 华为2288v3-2 | 172.16.2.21 | root    | Huawei12#$ | 默认从pxe boot |

可以用的dhcp网段为172.16.3.200-172.16.3.230

<!--more-->


## 安装 {#安装}

禁用selinux, `/etc/selinux/config`

```conf
SELINUX=disabled
```

```sh
setenforce 0
```

禁用防火墙

```sh
systemctl disable firewalld.service
systemctl stop firewalld
```

重启服务器

安装

-   epel-release
-   cobbler
-   cobbler-web
-   httpd
-   dnsmasq
-   xinetd
-   resync
-   fen-agents
-   debmirror
-   pykickstart

```sh
yum install -y epel-release
```

```sh
yum install -y cobbler cobbler-web httpd dnsmasq xinetd fence-agents debmirror pykickstart
```

修改 `/etc/cobbler/dnsmasq` 的配置

```conf
dhcp-range=172.16.3.200,172.16.3.230
```

生成密码, 这里的密码是 `Pioneer@2016`

```sh
[root@localhost cobbler]# openssl passwd -1
Password:
Verifying - Password:
$1$LT0SpDmj$k/UXh9oPX9AwZWWkDd6L./
```

修改 `/etc/cobbler/settings` 的配置

```conf
# set to 1 to enable Cobbler's DHCP management features.
# the choice of DHCP management engine is in /etc/cobbler/modules.conf
manage_dhcp: 1

# set to 1 to enable Cobbler's DNS management features.
# the choice of DNS mangement engine is in /etc/cobbler/modules.conf
manage_dns: 1

# cobbler has various sample kickstart templates stored
# in /var/lib/cobbler/kickstarts/.  This controls
# what install (root) password is set up for those
# systems that reference this variable.  The factory
# default is "cobbler" and cobbler check will warn if
# this is not changed.
# The simplest way to change the password is to run
# openssl passwd -1
# and put the output between the "" below.
default_password_crypted: "$1$LT0SpDmj$k/UXh9oPX9AwZWWkDd6L./"


# if using cobbler with manage_dhcp, put the IP address
# of the cobbler server here so that PXE booting guests can find it
# if you do not set this correctly, this will be manifested in TFTP open timeouts.
next_server: 172.16.3.30

# set to 1 to enable Cobbler's RSYNC management features.
manage_rsync: 1

# this is the address of the cobbler server -- as it is used
# by systems during the install process, it must be the address
# or hostname of the system as those systems can see the server.
# if you have a server that appears differently to different subnets
# (dual homed, etc), you need to read the --server-override section
# of the manpage for how that works.
server: 172.16.3.30


# if this setting is set to 1, cobbler systems that pxe boot
# will request at the end of their installation to toggle the
# --netboot-enabled record in the cobbler system record.  This eliminates
# the potential for a PXE boot loop if the system is set to PXE
# first in it's BIOS order.  Enable this if PXE is first in your BIOS
# boot order, otherwise leave this disabled.   See the manpage
# for --netboot-enabled.
pxe_just_once: 1
```

修改 `/etc/cobbler/modules.conf`,

```conf
[dns]
#module = manage_bind
module = manage_dnsmasq

[dhcp]
#module = manage_isc
module = manage_dnsmasq
```

修改 `/etc/xinetd.d/tftp`

```conf
disable = no
```

修改 `/etc/debmirror.conf`, 注释掉这两行

```conf
#@dists="sid";
#@arches="i386";
```

启用服务

```sh
systemctl enable httpd xinetd rsyncd cobblerd dnsmasq
systemctl start httpd xinetd rsyncd cobblerd dnsmasq
```

获取loader

```sh
cobbler get-loaders
```

检查状态

```sh
cobbler check
# No configuration problems found.  All systems go.
```

没有问题了，安装完成


## 下载镜像 {#下载镜像}

下载两个镜像做测试:

```sh
wget -c http://mirrors.163.com/ubuntu-releases/16.04/ubuntu-16.04.3-server-amd64.iso
wget -c https://mirrors.ustc.edu.cn/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1708.iso
```


## 导入镜像 {#导入镜像}


### centos {#centos}

mount

```sh
mount -o loop,ro CentOS-7-x86_64-Minimal-1708.iso /mnt
```

import

```text
[root@centosbox ~]# cobbler import --name=Centos7 --arch=x86_64 --path=/mnt
task started: 2017-11-17_122923_import
task started (id=Media import, time=Fri Nov 17 12:29:23 2017)
Found a candidate signature: breed=redhat, version=rhel6
Found a candidate signature: breed=redhat, version=rhel7
Found a matching signature: breed=redhat, version=rhel7
```

检查一下是否导入

```text
[root@centosbox ~]# cobbler distro list
   Centos7-x86_64
[root@centosbox ~]# cobbler profile list
   Centos7-x86_64
```

查看详细信息：

```text
[root@localhost isos]# cobbler distro report --name=Centos7-x86_64
Name                           : Centos7-x86_64
Architecture                   : x86_64
TFTP Boot Files                : {}
Breed                          : redhat
Comment                        :
Fetchable Files                : {}
Initrd                         : /var/www/cobbler/ks_mirror/Centos7-x86_64/images/pxeboot/initrd.img
Kernel                         : /var/www/cobbler/ks_mirror/Centos7-x86_64/images/pxeboot/vmlinuz
Kernel Options                 : {}
Kernel Options (Post Install)  : {}
Kickstart Metadata             : {'tree': 'http://@@http_server@@/cblr/links/Centos7-x86_64'}
Management Classes             : []
OS Version                     : rhel7
Owners                         : ['admin']
Red Hat Management Key         : <<inherit>>
Red Hat Management Server      : <<inherit>>
Template Files                 : {}
```

在system中添加

```text
[root@localhost isos]# cobbler system add --name=test --profile=Centos7-x86_64
[root@localhost isos]#  cobbler system report --name=test
Name                           : test
TFTP Boot Files                : {}
Comment                        :
Enable gPXE?                   : <<inherit>>
Fetchable Files                : {}
Gateway                        :
Hostname                       :
Image                          :
IPv6 Autoconfiguration         : False
IPv6 Default Device            :
Kernel Options                 : {}
Kernel Options (Post Install)  : {}
Kickstart                      : <<inherit>>
Kickstart Metadata             : {}
LDAP Enabled                   : False
LDAP Management Type           : authconfig
Management Classes             : <<inherit>>
Management Parameters          : <<inherit>>
Monit Enabled                  : False
Name Servers                   : []
Name Servers Search Path       : []
Netboot Enabled                : True
Owners                         : <<inherit>>
Power Management Address       :
Power Management ID            :
Power Management Password      :
Power Management Type          : ipmitool
Power Management Username      :
Profile                        : Centos7-x86_64
Internal proxy                 : <<inherit>>
Red Hat Management Key         : <<inherit>>
Red Hat Management Server      : <<inherit>>
Repos Enabled                  : False
Server Override                : <<inherit>>
Status                         : production
Template Files                 : {}
Virt Auto Boot                 : <<inherit>>
Virt CPUs                      : <<inherit>>
Virt Disk Driver Type          : <<inherit>>
Virt File Size(GB)             : <<inherit>>
Virt Path                      : <<inherit>>
Virt PXE Boot                  : 0
Virt RAM (MB)                  : <<inherit>>
Virt Type                      : <<inherit>>
```


### 导入ubuntu {#导入ubuntu}

```sh
umount /mnt
mount -o loop,ro ubuntu-16.04.3-server-amd64.iso /mnt
cobbler import --name=ubuntu1604 --arch=x86_64 --path=/mnt
cobbler system add --name=testubuntu --profile=ubuntu1604-x86_64
```

```text
[root@localhost isos]# cobbler distro list
   Centos7-x86_64
   ubuntu1604-amd64-x86_64
   ubuntu1604-hwe-amd64-x86_64
```


## 开始验证 {#开始验证}

那第一台机器mac地址为 `9C:E3:74:FF:82:98`, 设置静态ip为 `172.16.3.201`

给装上centos7

```text
[root@localhost isos]# cobbler system edit --name=test --interface=eth0 --mac=9C:E3:74:FF:82:98 --ip-address=172.16.3.201 --netmask=255.255.255.0 --static=1 --dns-name=testcentos7.mydomain.com
[root@localhost isos]# cobbler system edit --name=test --gateway=172.16.3.1 --hostname=testcentos7.mydomain.com
```

第二台机器的mac地址为 `9C:E3:74:42:61:21`, 设置静态ip为 `172.16.3.202`, 装上ubuntu

```sh
cobbler system edit --name=testubuntu --interface=eth0 --mac=9C:E3:74:42:61:21 --ip-address=172.16.3.202 --netmask=255.255.255.0 --static=1 --dns-name=testubuntu1604.mydomain.com
cobbler system edit --name=testubuntu --gateway=172.16.3.1 --hostname=testubuntu1604.mydomain.com
```

安装出问题了, 修改profile为centos一切正常

```sh
cobbler system edit --name=testubuntu --profile=Centos7-x86_64
```
