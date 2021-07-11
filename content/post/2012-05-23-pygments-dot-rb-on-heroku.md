+++
title = "pygments.rb on heroku"
date = "2012-05-23T17:27:00+08:00"
comments = true
categories = ["programming"]
tags = ["octopress"]
description = ""
+++


如何在`heroku`中使用`pygments.rb`呢？在`plugin/pygments_code.rb`中添加：

```ruby
require 'rubypython'
RubyPython.start(:python_exe => "python2.6")
```
<!--more-->