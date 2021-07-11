+++
title = "将 hexo 从2.8.3升级到了3.1.1"
tags = ["programming"]
categories = ["programming"]
date = "2015-08-07T14:23:00+08:00"
description = ""
+++


今天发现 blog 的 travis 编译貌似出问题了，很多东西都编译失败，所以尝试将 hexo 从2.8.3升级到了3.1.1，过程很顺利，通过官方文档就可以办到。

完成之后加入了[hexo-ruby-character](https://github.com/JamesPan/hexo-ruby-character)这个东东，安装

```sh
npm install hexo-ruby-character --save
```

测试一下:

> {% ruby Base|top %}
> {% ruby 佐天泪子|掀裙狂魔 %}
> {% ruby 超電磁砲|レールガン %}


是不是很酷:)

添加一下数学公式的支持吧，安装[hexo-math](https://github.com/akfish/hexo-math)

```sh
npm install hexo-math --save
```

测试一下

>$$\frac{\partial u}{\partial t}
>= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
>\frac{\partial^2 u}{\partial y^2} +
>\frac{\partial^2 u}{\partial z^2}\right)$$


> {% math_block %} \begin{aligned} \dot{x} & = \sigma(y-x) \\ \dot{y} & = \rho x - y - xz \\ \dot{z} & = -\beta z + xy \end{aligned} {% endmath_block %}

最后来个牛逼的吧，薛定谔方程，大学物理考试貌似还复习过这个公式，虽然现在已经记不清是什么意思来着了：

$$ i\hbar\frac{\partial \psi}{\partial t} = \frac{-\hbar^2}{2m} \left( \frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2} + \frac{\partial^2}{\partial z^2} \right) \psi + V \psi.$$




<!--more-->