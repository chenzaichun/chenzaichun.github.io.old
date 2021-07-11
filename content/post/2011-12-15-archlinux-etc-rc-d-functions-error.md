+++
categories = ["emacs", "Linux", "org2blog"]
comments = true
excerpt = "linux, archlinux, emacs, org2blog, programming"
date = "2011-12-15T10:23:33+08:00"
published = true
status = "publish"
tags = ["emacs", "Linux", "org2blog"]
title = "archlinux functions脚本错误"
type = "post"
description = ""
+++


今天升级了archlinux，启动过程中出现了错误：

```
/etc/rc.d/functions: line 497: syntax error near unexpected token `<'
/etc/rc.d/functions: line 497: `        done < <(findmnt -runRo TARGET,FSTYPE,OPTIONS / | tac)'
```

archlinux.org上给出了一个[patch](https://bugs.archlinux.org/task/27098)

patch的内容如下：
```diff
--- functions_old       2011-11-19 13:05:47.921522255 +0400
+++ functions   2011-11-19 12:55:17.411565127 +0400
@@ -494,7 +494,7 @@
                fi

                mounts+=("$target")
-       done < <(findmnt -runRo TARGET,FSTYPE,OPTIONS / | tac)
+       done < $(findmnt -runRo TARGET,FSTYPE,OPTIONS / | tac)

        umount -r "${mounts[@]}"
```

下载[patch](/media/wpid-rc-functions-typo.patch_.zip)
<!--more-->