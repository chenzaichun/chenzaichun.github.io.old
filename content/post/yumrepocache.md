+++
title = "yum/deb repository cache"
date = 2017-11-28
lastmod = 2017-12-04T20:23:45+08:00
tags = ["hugo", "org", "linux"]
categories = ["emacs", "tools"]
draft = false
+++

怎么做cache，参考了以下方式：

-   [squid](http://serverascode.com/2014/03/29/squid-cache-yum.html)
-   [nginx](http://tdt.rocks/repo_cache_ft_nginx.html)
-   [apt-mirror](https://www.packtpub.com/books/content/create-local-ubuntu-repository-using-apt-mirror-and-apt-cacher) Apt-Cacher
-   [python pip cache](https://packaging.python.org/guides/index-mirrors-and-caches/)
-   [python cache](http://www.dctrwatson.com/2013/06/simple-pypi-caching-proxy/)
-   [nginx like squid](http://sudhaker.com/60/caching-yum-proxy)
-   [ansible with vagrant](https://github.com/mrmoje/repo_cache_plays_ft_nginx)

<!--more-->

```nginx
# The upstream directive defining the backend/upstream repo hosts
# see http://nginx.org/en/docs/http/ngx_http_upstream_module.html
# There is a primary and a backup. You can have more than that.
# NGINX will try to load balance between them using a "weighted round-robin"
# algorithm.
# Most requests go to the primary, fewer to the rest.
# Error retries trickle down the server definitions. If the last errs,
# the result is passed on to the client.
# By default I found that if one is unreachable during startup, nginx will
# kaput leaving clues in the error log.
upstream ubuntu {
  server mirrors.ustc.edu.cn;
  server mirrors.163.com backup;
}

upstream centos {
    server mirrors.ustc.edu.cn;
    server mirrors.163.com backup;
}

# The following directive configures the cache. Storage et all. You may want
# the storage location  at a mount point with a filesystem on separate storage
# device/backend to match.
# see http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path.
proxy_cache_path /var/repo_mirror # defines where the cache is stashed

        # defines cache path heirarchy (yaani num directory levels in cache)
        levels=1:2

        # defines name and size of zone where all cache keys and cache metadata are stashed.
        # Servers as a lookup for cached data
        keys_zone=repository_cache:50m

        # The cached data access timeout. Pkgs get nuked if no access in 14 days.
        inactive=14d

        # Cache size limit
        max_size=200g;

# Our no-name server block
server {

  # Keep our eyes peeled on port 80
  listen 80;

  # Location directive for the /ubuntu path
  location /ubuntu {
    # our cache's root
    root /var/repo_mirror/index_data;

    # look for packages in the following order
    try_files $uri @ubuntu;
  }

  # Location directive for the named location defined above
  location @ubuntu {

    # map this to the upstream definition above
    proxy_pass http://ubuntu;

    # 14days of caching for http code 200 response content
    proxy_cache_valid 200 14d;

    # we set our "repository_cache" zone for caching
    proxy_cache repository_cache;

    # Use stale cached data in the error events defined
    proxy_cache_use_stale error timeout invalid_header updating;

    # pass request to next (backup) server in the error events defined
    proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
  }

  # Location directive for the /ubuntu path
  location /centos {
      # our cache's root
      root /var/repo_mirror/index_data;

      # look for packages in the following order
      try_files $uri @centos;
  }

  # Location directive for the named location defined above
  location @centos {

      # map this to the upstream definition above
      proxy_pass http://centos;

      # 14days of caching for http code 200 response content
      proxy_cache_valid 200 14d;

      # we set our "repository_cache" zone for caching
      proxy_cache repository_cache;

      # Use stale cached data in the error events defined
      proxy_cache_use_stale error timeout invalid_header updating;

      # pass request to next (backup) server in the error events defined
      proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
  }

}
```

首先拿ubuntu来试，发现死活不行：

```text
Fetched 415 kB in 23s (17.9 kB/s)
Reading package lists... Done
W: The repository 'http://mirrors.rc.com/ubuntu xenial Release' does not have a Release file.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
W: The repository 'http://mirrors.rc.com/ubuntu xenial-updates Release' does not have a Release file.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
E: Failed to fetch http://mirrors.rc.com/ubuntu/dists/xenial-backports/main/source/Sources  404  Not Found
E: Failed to fetch http://mirrors.rc.com/ubuntu/dists/xenial/main/source/Sources  404  Not Found
E: Failed to fetch http://mirrors.rc.com/ubuntu/dists/xenial-updates/main/source/Sources  404  Not Found
E: Some index files failed to download. They have been ignored, or old ones used instead.
```

还是不行，升级一下nginx试一下

```sh
add-apt-repository ppa:nginx/stable
apt-get update
apt-get install nginx
```

还是不行，网上怎么行的？？？？

直接用docker试试看。

没有分析日志的结果，哎，貌似是国内源不允许做upstream的。

发现这个： <https://pulpproject.org/>

这里有常用方式的介绍： <http://yum.baseurl.org/wiki/YumMultipleMachineCaching>

试试squid的方式:

```sh
docker run \
       --name lazy-distro-mirrors \
       -d \
       -p 80:8080 \
       -v `pwd`/config:/docker_configurator/user \
       -v `pwd`/cache:/var/spool/squid3 \
       enigmacurry/lazy-distro-mirrors
```

都是同样的问题呢？？？

```text
Reading package lists... Done
W: The repository 'http://mirrors.rc.com/ubuntu xenial Release' does not have a Release file.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
W: The repository 'http://mirrors.rc.com/ubuntu xenial-updates Release' does not have a Release file.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
W: The repository 'http://mirrors.rc.com/ubuntu xenial-backports Release' does not have a Release file.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
E: Failed to fetch http://mirrors.rc.com/ubuntu/dists/xenial/main/source/Sources  Connection failed
E: Failed to fetch http://mirrors.rc.com/ubuntu/dists/xenial-updates/main/source/Sources  Connection failed
E: Failed to fetch http://mirrors.rc.com/ubuntu/dists/xenial-backports/main/source/Sources  Connection failed
E: Some index files failed to download. They have been ignored, or old ones used instead.
```

终于通了，真的方式是这样的，

-   修改mirror的地址比如: mirrors.ustc.edu.cn
-   修改/etc/hosts, 添加 mirrors.ustc.edu.cn -> 10.69.0.2
-   修改nginx的配置，添加 server\_name -> mirrors.ustc.edu.cn \*.ubuntu.com ....


## yum 使用createrepo方式来创建 {#yum-使用createrepo方式来创建}

<https://www.digitalocean.com/community/tutorials/how-to-set-up-and-use-yum-repositories-on-a-centos-6-vps>

```sh
mkdir -p /mysql && cd /mysql
wget -c https://mirrors.ustc.edu.cn/mysql-repo/yum/mysql-5.7-community/el/7/x86_64/mysql-community-server-5.7.19-1.el7.x86_64.rpm
createrepo /mysql
```

```sh
yum --disablerepo="*" --enablerepo="docker-ce-stable" list available|grep libselinux-python
```

查看deps

```text
[root@cmp-vi650rc4 mysql]# rpm -qpR ./mysql-community-server-5.7.19-1.el7.x86_64.rpm
警告：./mysql-community-server-5.7.19-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
/bin/bash
/bin/sh
/bin/sh
/bin/sh
/bin/sh
/usr/bin/perl
config(mysql-community-server) = 5.7.19-1.el7
coreutils
grep
ld-linux-x86-64.so.2()(64bit)
ld-linux-x86-64.so.2(GLIBC_2.3)(64bit)
libaio.so.1()(64bit)
libaio.so.1(LIBAIO_0.1)(64bit)
libaio.so.1(LIBAIO_0.4)(64bit)
libc.so.6()(64bit)
libc.so.6(GLIBC_2.10)(64bit)
libc.so.6(GLIBC_2.14)(64bit)
libc.so.6(GLIBC_2.15)(64bit)
libc.so.6(GLIBC_2.16)(64bit)
libc.so.6(GLIBC_2.17)(64bit)
libc.so.6(GLIBC_2.2.5)(64bit)
libc.so.6(GLIBC_2.3)(64bit)
libc.so.6(GLIBC_2.3.4)(64bit)
libc.so.6(GLIBC_2.4)(64bit)
libc.so.6(GLIBC_2.7)(64bit)
libc.so.6(GLIBC_2.8)(64bit)
libcrypt.so.1()(64bit)
libcrypt.so.1(GLIBC_2.2.5)(64bit)
libdl.so.2()(64bit)
libdl.so.2(GLIBC_2.2.5)(64bit)
libgcc_s.so.1()(64bit)
libgcc_s.so.1(GCC_3.0)(64bit)
libgcc_s.so.1(GCC_3.3.1)(64bit)
libgcc_s.so.1(GCC_3.4)(64bit)
libm.so.6()(64bit)
libm.so.6(GLIBC_2.2.5)(64bit)
libnuma.so.1()(64bit)
libnuma.so.1(libnuma_1.1)(64bit)
libnuma.so.1(libnuma_1.2)(64bit)
libpthread.so.0()(64bit)
libpthread.so.0(GLIBC_2.12)(64bit)
libpthread.so.0(GLIBC_2.2.5)(64bit)
libpthread.so.0(GLIBC_2.3.2)(64bit)
librt.so.1()(64bit)
librt.so.1(GLIBC_2.2.5)(64bit)
librt.so.1(GLIBC_2.3.3)(64bit)
libsasl2.so.3()(64bit)
libstdc++.so.6()(64bit)
libstdc++.so.6(CXXABI_1.3)(64bit)
libstdc++.so.6(CXXABI_1.3.1)(64bit)
libstdc++.so.6(GLIBCXX_3.4)(64bit)
libstdc++.so.6(GLIBCXX_3.4.11)(64bit)
libstdc++.so.6(GLIBCXX_3.4.15)(64bit)
libstdc++.so.6(GLIBCXX_3.4.5)(64bit)
libstdc++.so.6(GLIBCXX_3.4.6)(64bit)
libstdc++.so.6(GLIBCXX_3.4.9)(64bit)
mysql-community-client(x86-64) >= 5.7.9
mysql-community-common(x86-64) = 5.7.19-1.el7
net-tools
perl(Getopt::Long)
perl(strict)
procps
rpmlib(CompressedFileNames) <= 3.0.4-1
rpmlib(FileDigests) <= 4.6.0-1
rpmlib(PayloadFilesHavePrefix) <= 4.0-1
rtld(GNU_HASH)
shadow-utils
systemd
systemd
systemd
rpmlib(PayloadIsXz) <= 5.2-1
```

安装 `repotrack`

```sh
yum install yum-utils
```

```sh
rpm -ivh http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

```sh
repotrack mysql-community-server.x86_64
```

脚本创建: <https://gist.github.com/alastori/f65de76039e11668396761e4b8ba7e7b>

```conf
## Enable to use MySQL 5.6 Commercial Server
[mysql56-commercial]
name=MySQL 5.6 Commercial Server
baseurl=$REPO_URL/mysql-5.6-commercial/el/7/$basearch/
enabled=0
gpgcheck=0
#gpgkey=$REPO_URL/RPM-GPG-KEY-mysql
```

```sh
createrepo el/7/x86_64/
```

看看ubuntu的方式:

<https://stackoverflow.com/questions/22008193/how-to-list-download-the-recursive-dependencies-of-a-debian-package>

```sh
apt-get download $(apt-cache depends --recurse --no-recommends --no-suggests --no-conflicts --no-breaks --no-replaces --no-enhances --no-pre-depends mysql-server mysql-client | grep "^\w" | sort -u)
dpkg-scanpackages . | gzip -9c > Packages.gz
```

<https://www.itzhoulin.com/2016/05/24/create-ubuntu-source-file-based-local-deb-files/>

```sh
apt-get install reprepro -y
reprepro --ask-passphrase -Vb . includedeb trusty deb/*.deb
```


## aptly {#aptly}


### 下载 {#下载}

```sh
wget -c https://bintray.com/artifact/download/smira/aptly/aptly_1.1.1_linux_amd64.tar.gz
tar xvf aptly_1.1.1_linux_amd64.tar.gz
cd aptly_1.1.1_linux_amd64 && cp aptly /usr/bin/
```

使用aptly: <https://www.aptly.info/tutorial/mirror/>


### gen key {#gen-key}

```sh
gpg --gen-key
```

```text
oot@cmp-k6czjga2:~/aptly/aptly_1.1.1_linux_amd64# gpg --gen-key
gpg (GnuPG) 1.4.16; Copyright (C) 2013 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection?
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N) y

You need a user ID to identify your key; the software constructs the user ID
from the Real Name, Comment and Email Address in this form:
    "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"

Real name: chenzaichun
Email address: chenzaichun@cloud-star.com.cn
Comment: Signing Repo
You selected this USER-ID:
    "chenzaichun (Signing Repo) <chenzaichun@cloud-star.com.cn>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
You need a Passphrase to protect your secret key.

gpg: gpg-agent is not available in this session
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
.+++++
..+++++
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
+++++
....+++++
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key 3FFC607E marked as ultimately trusted
public and secret key created and signed.

gpg: checking the trustdb
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
pub   2048R/3FFC607E 2017-12-01
      Key fingerprint = 4403 5E29 0227 B84D 0793  A72E 2254 C689 3FFC 607E
uid                  chenzaichun (Signing Repo) <chenzaichun@cloud-star.com.cn>
sub   2048R/0189E080 2017-12-01
```


### create mirror {#create-mirror}

```text
root@cmp-k6czjga2:~/aptly# aptly mirror create -architectures=amd64 -filter='Priority (required) | Priority (important) | Priority (standard)' trusty-main http://mirrors.ustc.edu.cn/ubuntu/ trusty main
Config file not found, creating default config at /root/.aptly.conf


Looks like your keyring with trusted keys is empty. You might consider importing some keys.
If you're running Debian or Ubuntu, it's a good idea to import current archive keys by running:

  gpg --no-default-keyring --keyring /usr/share/keyrings/debian-archive-keyring.gpg --export | gpg --no-default-keyring --keyring trustedkeys.gpg --import

(for Ubuntu, use /usr/share/keyrings/ubuntu-archive-keyring.gpg)

Downloading http://mirrors.ustc.edu.cn/ubuntu/dists/trusty/InRelease...
Downloading http://mirrors.ustc.edu.cn/ubuntu/dists/trusty/Release...
Downloading http://mirrors.ustc.edu.cn/ubuntu/dists/trusty/Release.gpg...
gpgv: Signature made Thursday, May 08, 2014 PM10:20:33 CST using DSA key ID 437D05B5
gpgv: Can't check signature: public key not found
gpgv: Signature made Thursday, May 08, 2014 PM10:20:33 CST using RSA key ID C0B21F32
gpgv: Can't check signature: public key not found

Looks like some keys are missing in your trusted keyring, you may consider importing them from keyserver:

gpg --no-default-keyring --keyring trustedkeys.gpg --keyserver keys.gnupg.net --recv-keys 40976EAF437D05B5 3B4FE6ACC0B21F32

Sometimes keys are stored in repository root in file named Release.key, to import such key:

wget -O - https://some.repo/repository/Release.key | gpg --no-default-keyring --keyring trustedkeys.gpg --import

ERROR: unable to fetch mirror: verification of detached signature failed: exit status 2
```

```text
root@cmp-k6czjga2:~/aptly# gpg --no-default-keyring --keyring /usr/share/keyrings/ --export | gpg --no-default-keyring --keyring trustedkeys.gpg --import
debian-archive-keyring.gpg       ubuntu-archive-keyring.gpg       ubuntu-archive-removed-keys.gpg  ubuntu-master-keyring.gpg
root@cmp-k6czjga2:~/aptly# gpg --no-default-keyring --keyring /usr/share/keyrings/ --export | gpg --no-default-keyring --keyring trustedkeys.gpg --import
debian-archive-keyring.gpg       ubuntu-archive-keyring.gpg       ubuntu-archive-removed-keys.gpg  ubuntu-master-keyring.gpg
root@cmp-k6czjga2:~/aptly# gpg --no-default-keyring --keyring /usr/share/keyrings/ubuntu-archive-keyring.gpg --export | gpg --no-default-keyring --keyring trustedkeys.gpg --import
gpg: key 437D05B5: public key "Ubuntu Archive Automatic Signing Key <ftpmaster@ubuntu.com>" imported
gpg: key FBB75451: public key "Ubuntu CD Image Automatic Signing Key <cdimage@ubuntu.com>" imported
gpg: key C0B21F32: public key "Ubuntu Archive Automatic Signing Key (2012) <ftpmaster@ubuntu.com>" imported
gpg: key EFE21092: public key "Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>" imported
gpg: Total number processed: 4
gpg:               imported: 4  (RSA: 2)
gpg: public key of ultimately trusted key 3FFC607E not found
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
```

```text
root@cmp-k6czjga2:~/aptly# gpg --no-default-keyring --keyring /usr/share/keyrings/ubuntu-master-keyring.gpg --export | gpg --no-default-keyring --keyring trustedkeys.gpg --import
gpg: key 3F272F5B: public key "Ubuntu Archive Master Signing Key <ftpmaster@ubuntu.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
gpg: public key of ultimately trusted key 3FFC607E not found
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
```

```sh
aptly mirror create -architectures=amd64 -filter='Priority (required) | Priority (important) | Priority (standard) | mysql-server | ngnix ' -filter-with-deps trusty-main http://mirrors.ustc.edu.cn/ubuntu/ trusty main
```

```text
root@cmp-k6czjga2:~/aptly# aptly mirror create -architectures=amd64 -filter='Priority (required) | Priority (important) | Priority (standard) | mysql-server | ngnix ' -filter-with-deps trusty-main http://mirrors.ustc.edu.cn/ubuntu/ trusty main
Downloading http://mirrors.ustc.edu.cn/ubuntu/dists/trusty/InRelease...
Downloading http://mirrors.ustc.edu.cn/ubuntu/dists/trusty/Release...
Downloading http://mirrors.ustc.edu.cn/ubuntu/dists/trusty/Release.gpg...
gpgv: Signature made Thursday, May 08, 2014 PM10:20:33 CST using DSA key ID 437D05B5
gpgv: Good signature from "Ubuntu Archive Automatic Signing Key <ftpmaster@ubuntu.com>"
gpgv: Signature made Thursday, May 08, 2014 PM10:20:33 CST using RSA key ID C0B21F32
gpgv: Good signature from "Ubuntu Archive Automatic Signing Key (2012) <ftpmaster@ubuntu.com>"

Mirror [trusty-main]: http://mirrors.ustc.edu.cn/ubuntu/ trusty successfully added.
You can run 'aptly mirror update trusty-main' to download repository contents.
```

Create also trusty-updates and trusty-security mirrors to get important updates:

```sh
aptly mirror create -architectures=amd64 -filter='Priority (required) | Priority (important) | Priority (standard) | mysql-server | mysql-client' -filter-with-deps trusty-updates http://mirrors.ustc.edu.cn/ubuntu/ trusty-updates main

aptly mirror create -architectures=amd64 -filter='Priority (required) | Priority (important) | Priority (standard) | mysql-server | mysql-client' -filter-with-deps trusty-security http://security.ubuntu.com/ubuntu trusty-security main

```


### update it {#update-it}

```sh
aptly mirror update trusty-main
aptly mirror update trusty-updates
aptly mirror update trusty-security
```


### snapshots {#snapshots}

```sh
aptly snapshot create trusty-main-12.1 from mirror trusty-main
aptly snapshot create trusty-updates-12.1 from mirror trusty-updates
aptly snapshot create trusty-security-12.1 from mirror trusty-security
```

```text
root@cmp-k6czjga2:~/aptly# aptly snapshot create trusty-main-12.1 from mirror trusty-main

Snapshot trusty-main-12.1 successfully created.
You can run 'aptly publish snapshot trusty-main-12.1' to publish snapshot as Debian repository.
root@cmp-k6czjga2:~/aptly#   aptly snapshot create trusty-updates-12.1 from mirror trusty-updates

Snapshot trusty-updates-12.1 successfully created.
You can run 'aptly publish snapshot trusty-updates-12.1' to publish snapshot as Debian repository.

Snapshot trusty-security-12.1 successfully created.
You can run 'aptly publish snapshot trusty-security-12.1' to publish snapshot as Debian repository.
root@cmp-k6czjga2:~/aptly#
```


### MERGING SNAPSHOTS {#merging-snapshots}

```sh
aptly snapshot merge -latest trusty-final-12.1 trusty-main-12.1 trusty-updates-12.1 trusty-security-12.1
```


### verify {#verify}

```text
root@cmp-k6czjga2:~/aptly# aptly package show -with-references mysql-server
Package: mysql-server
Priority: optional
Section: database
Installed-Size: 132
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Debian MySQL Maintainers <pkg-mysql-maint@lists.alioth.debian.org>
Architecture: all
Source: mysql-5.5
Version: 5.5.35+dfsg-1ubuntu1
Depends: mysql-server-5.5
Filename: mysql-server_5.5.35+dfsg-1ubuntu1_all.deb
Size: 12460
MD5sum: d16af3d1cdb56d9c901566a0f9a47a0a
SHA1: cc2ca722277fa1b00a32000985cffb6e188a798b
SHA256: 37dba3b3f7094813f557c3852d0891fef7959e3d10134fae2627aaa270c92ab2
SHA512: a40330b74bb45b7f974012e64bcf1c09659cd8d66c4d6b2a47afceef5173c1a1dad20d313ff461019c91f37f87296a1a3841c885e8a57b2523006ddd91da5739
Description: MySQL database server (metapackage depending on the latest version)
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Origin: Ubuntu
Homepage: http://dev.mysql.com/
Supported: 5y
Task: lamp-server, mythbuntu-frontend, mythbuntu-desktop, mythbuntu-backend-slave, mythbuntu-backend-master, mythbuntu-backend-master
Description-Md5: e519a9c4f87658afbd1d077af63f9269

References to package:
  mirror [trusty-main]: http://mirrors.ustc.edu.cn/ubuntu/ trusty
  snapshot [trusty-main-12.1]: Snapshot from mirror [trusty-main]: http://mirrors.ustc.edu.cn/ubuntu/ trusty

Package: mysql-server
Priority: optional
Section: database
Installed-Size: 125
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Debian MySQL Maintainers <pkg-mysql-maint@lists.alioth.debian.org>
Architecture: all
Source: mysql-5.5
Version: 5.5.58-0ubuntu0.14.04.1
Depends: mysql-server-5.5
Filename: mysql-server_5.5.58-0ubuntu0.14.04.1_all.deb
Size: 11272
MD5sum: 259d272c6d280266c51974504638dc9d
SHA1: 3be315aed287644449e4243625d8fb02d1fb222a
SHA256: ac571af3081af0781061d47d812b30c0cd16b9f8f8639e8af8f2e0bf74e93007
SHA512: e6379fc8a8c64dc194194f9939c98a52f86ebfaa100c5a52b67d2a1609a25a9bde7b4ddf83db89f63fdd554a92c21f3ab2a5dbffb2bf4d6f598458c77f8021c0
Description: MySQL database server (metapackage depending on the latest version)
Supported: 5y
Task: lamp-server, mythbuntu-frontend, mythbuntu-desktop, mythbuntu-backend-slave, mythbuntu-backend-master, mythbuntu-backend-master
Origin: Ubuntu
Description-Md5: e519a9c4f87658afbd1d077af63f9269
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Homepage: http://dev.mysql.com/

References to package:
  mirror [trusty-updates]: http://mirrors.ustc.edu.cn/ubuntu/ trusty-updates
  mirror [trusty-security]: http://security.ubuntu.com/ubuntu/ trusty-security
  snapshot [trusty-updates-12.1]: Snapshot from mirror [trusty-updates]: http://mirrors.ustc.edu.cn/ubuntu/ trusty-updates
  snapshot [trusty-final-12.1]: Merged from sources: 'trusty-main-12.1', 'trusty-updates-12.1', 'trusty-security-12.1'
  snapshot [trusty-security-12.1]: Snapshot from mirror [trusty-security]: http://security.ubuntu.com/ubuntu/ trusty-security
```


### PUBLISHING REPOSITORY {#publishing-repository}

```sh
aptly publish snapshot -distribution=trusty trusty-final-12.1
```


### server {#server}

```sh
aptly serve
```


### nginx backports {#nginx-backports}

<https://www.aptly.info/tutorial/pull/>

mirror script: <https://gist.github.com/EntropyWorks/94055445c19dabe6a870>

用这个版本来试试看：<https://github.com/smira/aptly/pull/608>
