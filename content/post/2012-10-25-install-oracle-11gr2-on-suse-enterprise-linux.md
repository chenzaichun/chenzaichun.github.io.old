+++
title = "SUSE Enterprise Linux 10下安装Oracle 11gr2"
date = "2012-10-25T10:09:00+08:00"
comments = true
categories = ["tools", "programming"]
tags = ["linux", "oracle", "suse"]
description = ""
+++


1. 首先准备好oracle的安装文件，11gr2默认有两个zip包：`linux.x64_11gR2_database_1of2.zip`和`linux.x64_11gR2_database_2of2.zip`, 将这两个包解压

```sh
unzip linux.x64_11gR2_database_1of2.zip
unzip linux.x64_11gR2_database_2of2.zip
```

2. 系统硬件需求这里忽略，详情查看oracle的文档

3. 所依赖的软件包

<!--more-->

```
binutils-2.16.91.0.5
compat-libstdc++-5.0.7
gcc-4.1.2
gcc-c++-4.1.2
glibc-2.4-31
glibc-devel-2.4-31
ksh-93r-12.9
libaio-0.3.104
libaio-devel-0.3.104
libelf-0.8.5
libgcc-4.1.2
libstdc++-4.1.2
libstdc++-devel-4.1.2
make-3.80
sysstat-8.0.4
```
安装可以通过`yast2`

```sh
yast2 -i <package list>
```

这里要注意，在suse enterprise linux中，`compat-libstdc++`由`libstdc++33`替代, 然后在check的时候忽略掉就行

```sh
yast2 -i libstdc++33
```

4. 修改内核参数`/etc/sysctl.conf`

```conf
kernel.sem = 1250 32000 100 256
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 4194304
net.core.rmem_max = 4194304
net.core.wmem_default = 1048576
net.core.wmem_max = 1048576
fs.file-max = 6815744
kernel.shmmax = 536870912
fs.aio-max-nr = 1048576
```

```sh
sysctl -p
```

5. 创建用户和组

```sh
groupadd oinstall
groupadd dba
useradd -g oinstall -G dba oracle
passwd oracle
...
```

6. 创建oracle安装目录

```sh
mkdir -p /opt/oracle
mkdir -p /home/oracle
mkdir -p /opt/oraInventory
chown -R oracle:oinstall /opt/oracle
chown -R oracle:oinstall /home/oracle
chown -R oracle:oinstall /opt/oraInventory
```

7. 修改oracle的环境变量, 添加一下变量到`~oracle/.profile`

```sh
ORACLE_BASE=/opt/oracle
ORACLE_HOME=$ORACLE_BASE/product/11.2.0 
export ORACLE_BASE ORACLE_HOME
PATH=$PATH:/$ORACLE_HOME/bin
export PATH
```

8. 修改`/etc/security/limits.conf`

```conf
oracle soft nproc 16384
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
```

9. 开始安装

```sh
su - oracle
cd <ORACLE_INSTALL_MEDIA_DIR>
./runInstaller
...
```

剩下的就是根据图形界面一步一步的做就行了 :)
