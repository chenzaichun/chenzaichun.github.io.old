+++
categories = ["emacs", "Linux", "org2blog", "programming", "tools"]
comments = true
date = "2011-11-18T21:10:33+08:00"
published = true
status = "publish"
tags = ["DVCS", "emacs", "git", "Linux", "org2blog", "VCS"]
title = "mount –bind"
type = "post"
description = ""
+++


我想应该大家都会用版本管理来处理配置文件吧。本人一般用git来管理自己的配置（emacs, vim, ssh, zshrc, svn, git, hg…），我将这写配置文件放到了一个git repo中。但是很多时候需要对这些文件进行修改，我想大家也不会把文件在repo中修改好之后然后手动copy到相应的目录中。这里一般有两种方法来避免手动拷贝，一种是通过Makefile的方式，另外一种就是建立文件的软链接。我原来一直采用的是后者，因为我不想没一次修改都make一下。对于建立软链接的方式，一般的软件都能很好的处理。   我的git repo放在windows分区（这样windows和linux都能使用），用virtualbox安装了archlinux，通过mount vboxsf的方式mount windows分区到linux下。这里mount vboxsf的时候需要注意权限问题，然后像.ssh/config文件会出现访问权限问题。我的 =/etc/fstab= 文件如下: 

```conf
# /etc/fstab
sharename mountpoint vboxsf \
rw,fmode=0644,dmode=0755,uid=1000,gid=1000,exec 0 0
```

对于symlink模式，绝大部分软件都能满足了，但是这里有个意外, `Oracle`的`gethostbyname`对于`/etc/hosts` symlinks是不认的，比如我创建一个软链接到`/etc/hosts`，启动oracle会出现
```
ORA-00600 arguments: [keltnfy-ldmInit], [46], [1]
```

既然软链接不行，这个时候就需要`mount –bind`的帮助了。   `mount –bind`是将一个目录中的内容挂载到另一个目录上，用法是:

```sh
mount --bind olddir newdir
mount --bind olddir/file newdir/file
```

如果想将`mount –bind`写入`/etc/fstab`，则用法是：

```sh
olddir newdir none bind 0 0
```

这里又有一个问题了，我如果想把/etc/fstab也放到git repo中进行管理怎么办呢？由于不可能为/etc/fstab创建symlink，也不能为其使用mount –bind的方式到/etc/fstab。那就只能在git repo中想办法了。可是git是不会follow symlink的，如果我将/etc/fstab软链接到git repo中，git并不会将该文件加入到repo，加入的只是一个symlink而已。   解决办法就是反过来bind, 将/etc/fstab bind到git repo中的目录:) 
<!--more-->