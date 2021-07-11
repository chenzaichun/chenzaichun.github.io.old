+++
title = "Floating point exception on SUSE"
date = "2012-10-26T16:40:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["suse", "redhat", "linux"]
description = ""
+++


今天想将redhat上面的程序放到suse 10上面运行，结果遇到了这个错误：

> Floating point exception

ldd的显示结果：

```sh
# on suse
ldd /usr/bin/pkginfo
	linux-vdso.so.1 =>  (0x00007fff1c7fe000)
	libc.so.6 => /lib64/libc.so.6 (0x00002afe8e51c000)
	/lib64/ld-linux-x86-64.so.2 (0x00002afe8e400000)

# on redhat
ldd /usr/bin/pkginfo
	libc.so.6 => /lib64/libc.so.6 (0x0000003dd0800000)
	/lib64/ld-linux-x86-64.so.2 (0x0000003dd0400000)
```

系统版本:

```sh
# on suse
uname -a
Linux vmsuse 2.6.16.60-0.54.5-default #1 Fri Sep 4 01:28:03 UTC 2009 x86_64 x86_64 x86_64 GNU/Linux

# on redhat
uname -a
Linux temipll1 2.6.18-92.el5 #1 SMP Tue Apr 29 13:16:15 EDT 2008 x86_64 x86_64 x86_64 GNU/Linux
```

gcc版本：

<!--more-->

```sh
# on suse 
gcc -v
Using built-in specs.
Target: x86_64-suse-linux
Configured with: ../configure --enable-threads=posix --prefix=/usr --with-local-prefix=/usr/local --infodir=/usr/share/info --mandir=/usr/share/man --libdir=/usr/lib64 --libexecdir=/usr/lib64 --enable-languages=c,c++,objc,fortran,obj-c++,java,ada --enable-checking=release --with-gxx-include-dir=/usr/include/c++/4.1.2 --enable-ssp --disable-libssp --disable-libgcj --with-slibdir=/lib64 --with-system-zlib --enable-shared --enable-__cxa_atexit --enable-libstdcxx-allocator=new --program-suffix= --enable-version-specific-runtime-libs --without-system-libunwind --with-cpu=generic --host=x86_64-suse-linux
Thread model: posix
gcc version 4.1.2 20070115 (SUSE Linux)

# on redhat 
gcc -v
Using built-in specs.
Target: x86_64-redhat-linux
Configured with: ../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-libgcj-multifile --enable-languages=c,c++,objc,obj-c++,java,fortran,ada --enable-java-awt=gtk --disable-dssi --enable-plugin --with-java-home=/usr/lib/jvm/java-1.4.2-gcj-1.4.2.0/jre --with-cpu=generic --host=x86_64-redhat-linux
Thread model: posix
gcc version 4.1.2 20080704 (Red Hat 4.1.2-48)
```

glibc 版本:

```sh
# on suse
rpm -qa|grep glibc
glibc-i18ndata-2.4-31.74.1
glibc-32bit-2.4-31.74.1
glibc-2.4-31.74.1
glibc-locale-32bit-2.4-31.74.1
glibc-locale-2.4-31.74.1
glibc-devel-32bit-2.4-31.74.1
glibc-devel-2.4-31.74.1

# on redhat
rpm -qa|grep glibc
glibc-2.5-49
glibc-devel-2.5-49
glibc-headers-2.5-49
glibc-common-2.5-49
glibc-2.5-49
glibc-devel-2.5-49
```

貌似suse和redhat的binay是不兼容的呢 - -||

用hello world程序测试一下:

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
	printf("Hello World\n");
	return 0;
}
```

在redhat上面编译

```sh
gcc helloworld.c -o helloworld
```

拷贝到suse上面运行，结果还是出现`Floating point exception`

Google了一下，发现可以在编译的时候加入`-Wl,--hash-style=sysv`

```sh
gcc helloworld.c -Wl,--hash-style=sysv -o helloworld
```

拷贝到suse上面运行竟然正常了。

Google发现了这样一段话

> Fedora (RedHat?) seems to have a different default dynamic linker hash style,
> I think basically it is:
>    ld --hash-style=gnu ..

> So without using one of:
>   --hash-style=both or --hash-style=sysv

> The bits created won't run on systems that don't support this new hash
> style, like SuSE 10.

OMG
