+++
title = "让IE浏览器支持CSS3圆角属性"
date = "2012-05-31T22:45:00+08:00"
comments = true
categories = ["programming"]
tags = ["octopress", "css3"]
description = ""
+++


IE6/7/8这三个IE浏览器版本都不支持CSS3的解析，只有还不太主流的IE9支持CSS3和HTML5的标准。

为了解决这个问题，去[这里](http://fetchak.com/ie-css3/)下载.htc [^1]文件，并将该文件放到网站根目录下。[参考链接](http://www.daqianduan.com/ie6yuanjiao/)

`Webkit`内核的浏览器支持`-webkit-border-radius: 10px;`属性（10px是圆角半径），可以直接解析出圆角；`Firefox`浏览器支持`-moz-border-radius: 10px;`属性，也是可以直接解析出圆角；`IE 系浏览器则需要加上`border-radius: 15px;`的属性。

所有在需要圆角的地方加入： 

<!--more-->

```css
.xxx {
    ...
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    border-radius: 10px;
    position:relative;
    z-index:2;
    behavior: url(此处为ie-css3.htc文件的绝对路径);
    ...
}
```

1. `behavior`的`url`里一定要填写`ie-css3.htc`的绝对路径，因为IE浏览器找该文件是相对当前html文件路径来找的，所以对于Wordpress等动态程序生成的页面一定要填写绝对路径。

2. 一定要有定位属性：`position:relative`;

3. 因为在IE浏览器下这些`CSS3`效果的实现是要借助于`VML`，由`VML`绘制圆角或是投影效果，所以还需要一个`z-index`属性。`z-index`属性最好设置得比较大，如2。

4. 如果在IE浏览器下某些模块无法用此渲染，可以试着绝对定位相应的层，即加上`width: 400px; height:400px;`属性。

5. `radius`属性的`10px`是圆角半径，还可以给两个值如`border-radius: 10px 5px;`，这样则左上角与右下角半径为10px，右上角与左下角半径为5px。也可以赋4个值，为“上  右  下  左”。

以上代码就是加入圆角支持的:)

[^1]: .htc文件是IE内核支持Web行为后用来描述此类行为的脚本文件。它们定义了一套方法和属性，程序员几乎可以把这些方法和属性应用到HTML页面上的任何元素上去。Web 行为是非常伟大的因为它们允许程序员把自定义的功能“连接”到现有的元素和控件，而不是必须让用户下载二进制文件（例如ActiveX 控件）来完成这个功能。 


