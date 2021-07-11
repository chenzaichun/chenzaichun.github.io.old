+++
title = "xiami自动签到"
date = "2012-05-23T13:10:00+08:00"
comments = true
categories = ["programming"]
tags = ["python"]
description = ""
+++


有的时候需要在虾米上面下点音乐，但是积分经常不够- -||。虾米提供了签到的方式来获取`红包`

对于虾米自动签到，目前有两种方式：

1. [虾米自动签到(喂食)系统](http://checkin.isombyt.me/)， 该网站建立在`GAE`上，不用设置直接提交就可以帮你自动签到，但是每天只能满足1400个人（不知道现在有多少人使用该系统），对于该系统的介绍和限制，参见其[blog](http://isombyt.me/2012/05/xiami-checkin/)
2. 另外一种是通过`python`脚本加`cron`的方式来达到，但是需要python的Linux/Unix服务器，参见[HuXuan](http://huxuan.org/projects/xiami_auto_checkin/)

我将自动该自动签到脚本放到了dotcloud上面，以下是我的配置：

1. ssh到dotcloud上：

```sh
dotcloud ssh
```

<!--more-->

2. 将以下代码放到`home`目录下, `~/xiami_auto_checkin.py`
```python
#!/usr/bin/python
# encoding:utf-8
# +-----------------------------------------------------------------------------
# | File: xiami_auto_checkin.py
# | Author: huxuan
# | E-mail: i(at)huxuan.org
# | Created: 2012-12-11
# | Last modified: 2012-02-06
# | Description:
# |     Description for xiami_auto_checkin.py
# |
# | Copyrgiht (c) 2012 by huxuan. All rights reserved.
# | License GPLv3
# +-----------------------------------------------------------------------------

import re
import os
import sys
import urllib
import urllib2
import datetime
import cookielib

def check(response):
    """Check whether checkin is successful

    Args:
        response: the urlopen result of checkin

    Returns:
        If succeed, return a string like '已经连续签到**天'
            ** is the amount of continous checkin days
        If not, return False
    """
    pattern = re.compile(r'<div class="idh">(已连续签到\d+天)</div>')
    result = pattern.search(response)
    if result: return result.group(1)
    return False

def main():
    """Main process of auto checkin
    """
    # Get log file
    LOG_DIR = os.path.join(os.path.expanduser("~"), '.log')
    if not os.path.isdir(LOG_DIR):
        os.makedirs(LOG_DIR)
    LOG_PATH = os.path.join(LOG_DIR, 'xiami_auto_checkin.log')
    f = LOG_FILE = file(LOG_PATH, 'a')
    print >>f # add a blank space to seperate log

    # Get email and password
    if len(sys.argv) != 3:
        print >>f, '[Error] Please input email & password as sys.argv!'
        print >>f, datetime.datetime.now()
        return
    email = sys.argv[1]
    password = sys.argv[2]

    # Init
    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookielib.CookieJar()))
    urllib2.install_opener(opener)

    # Login
    login_url = 'http://www.xiami.com/web/login'
    login_data = urllib.urlencode({'email':email, 'password':password, 'LoginButton':'登陆',})
    login_headers = {'Referer':'http://www.xiami.com/web/login', 'User-Agent':'Opera/9.60',}
    login_request = urllib2.Request(login_url, login_data, login_headers)
    login_response = urllib2.urlopen(login_request).read()

    # Checkin
    checkin_pattern = re.compile(r'<a class="check_in" href="(.*?)">')
    checkin_result = checkin_pattern.search(login_response)
    if not checkin_result:
        # Checkin Already | Login Failed
        result = check(login_response)
        if result:
            print >>f, '[Succeed] Checkin Already!', email, result
        else:
            print >>f, '[Error] Login Failed!', email
        print >>f, datetime.datetime.now()
        return
    checkin_url = 'http://www.xiami.com' + checkin_result.group(1)
    checkin_headers = {'Referer':'http://www.xiami.com/web', 'User-Agent':'Opera/9.60',}
    checkin_request = urllib2.Request(checkin_url, None, checkin_headers)
    checkin_response = urllib2.urlopen(checkin_request).read()

    # Result
    result = check(checkin_response)
    if result:
        print >>f, '[Succeed] Checkin Succeed!', email, result
    else:
        print >>f, '[Error] Checkin Failed!'
    print >>f, datetime.datetime.now()

if __name__=='__main__':
    main()
```
3. 执行`crontab -e`

```sh
crontab -e
```

4. 根据自身情况添加
```conf
# replace USERNAME and PASSWORD with email account and corresponding password
5 */8 * * * python /home/dotcloud/xiami_auto_checkin.py USERNAME PASSWORD
```

5. 保存退出
搞定
