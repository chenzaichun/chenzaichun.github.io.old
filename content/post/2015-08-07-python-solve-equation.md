+++
title = "用 Python 解方程"
tags = ["programming", "python"]
categories = ["programming"]
date = "2015-08-07T15:23:00+08:00"
description = ""
+++


今天无意中发现一张图片，

![WIFI密码](/media/wifi_password.jpg)

当如可以通过数学计算网站计算得到结果[symbolab](https://www.symbolab.com/solver/logarithmic-equation-calculator/log_%7B2%7D%5Cleft(x-1%5Cright)%2Blog_%7B2%7DX%3D1/?origin=enterkey)

当时病犯了，不承认自己是学渣（貌似我也不是学霸），学过的数学也完全还给了老师，肿么办。只好用 Python 中的[SymPy](http://www.sympy.org/en/index.html)来计算一下了以下方程

{% math_block %}
\log_2(x-1) + \log_2x = 1
{% endmath_block %}

安装`SymPy`

```sh
pip install sympy
```

求解，直接上代码

```python
#!/usr/bin/env python

from sympy import *

var('x')
print(solve(log(x-1,2)+log(x,2)-1,x))
```

结果是2，可是貌似还有一个负数解呢( `-1`  哪儿去了?)

<!-- more -->

```python
#!/usr/bin/env python

from sympy import *

var('x')
print(solve(log(x-1,2)+log(x,2)-1,x))

y = Symbol('y', negative=True)
print(solve(log(y-1,2)+log(y,2)-1,y))
```

暂时只能通过这种方式求解了。







<!--more-->