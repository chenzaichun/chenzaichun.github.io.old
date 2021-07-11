+++
categories = ["programming"]
comments = true
published = true
date = "2010-05-31T11:12:33+08:00"
status = "publish"
tags = ["boost", "Linux"]
title = "boost::progress_display"
type = "post"
description = ""
+++


boost::progress_display是个好玩的东东,用来现实一个进度条。测试代码：

```cpp
#include <boost/progress.hpp>

using namespace boost;

int main()
{
    progress_display p(10);     
    for (int i = 0; i < 10; ++i) {
        sleep(1);
        ++p;
    }

    return 0;
}
```

运行结果:

```sh
$ ./boostprogress

0%   10   20   30   40   50   60   70   80   90   100%
|----|----|----|----|----|----|----|----|----|----|
****************************************
```
<!--more-->