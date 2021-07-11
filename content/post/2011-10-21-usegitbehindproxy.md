+++
categories = ["无所事事"]
comments = true
published = true
status = "publish"
date = "2011-10-21T11:11:11+08:00"
tags = ["无所事事", "转载"]
title = "git代理设置"
type = "post"
description = ""
+++


Since someone is asking me to provide an English version about git proxy configuration in my former blog. Here it is. 1). Basically, I know two ways to use git behind a proxy.
 
1. Set the http_proxy environment if git repository supports http connections. 

find a proxy command to bypass the connection: <a href="https://bitbucket.org/chenzaichun/org2blog/src/e82a0244f078/media/connect.c">connect.c</a> 

```sh
gcc -o connect connect.c
mv connect ~/bin
```
    
2. Set up a wrapper 

```sh
echo "/home/gigi/bin/connect -H proxy.bupt.edu.cn:8080 $@" >> ~/bin/proxy
chmod +x ~/bin/proxy
```
        
Here I'm using a http proxy offered by my school, which uses port 8080     The connect command also support socket proxy. Use -S host:port to indicate that. 

3. Update bashrc 

```sh
echo "export CONNECT_USER=bergwolf" >> .bashrc
echo "export GIT_PROXY_COMMAND=proxexport GIT_PROXY_COMMAND=proxy" >> .bashrc
```
    
> The connect command reads proxy username and password from `CONNECT_USER` and `CONNECT_PASSWORD` evironment. The default username is current login user if no CONNECT_USER is set. Password will be requested interactively if CONNECT_PASSWORD is empty.   GIT_PROXY_COMMAND indicates that git should use the command "proxy"(the wrapper we setup at step 2) as its proxy_command. This can also be set in .git/config. 

Now, everything we need is done. Have a try: 

``` 
[gigi-Ubuntu:bin]$git clone git://git.kernel.org/pub/scm/fs/ext2/e2fsprogs.git
Initialized empty Git repository in /home/gigi/bin/e2fsprogs/.git/
Enter proxy authentication password for bergwolf@proxy.bupt.edu.cn:
remote: Counting objects: 24006, done.
remote: Compressing objects: 100% (4701/4701), done.
ceiving objects: 1% (241/24006), 43.99 KiB | 56 KiB/s
```
     
<!--more-->