+++
categories = ["programming"]
comments = true
published = true
date = "2010-06-03T11:37:11+08:00"
status = "publish"
tags = ["boost"]
title = "让boost::program_options支持未注册的参数"
type = "post"
description = ""
+++


如果在使用boost::program_options的时候传递了未注册的参数，则会throw exception，要想无视我们不需要的参数，可以通过使用`basic_command_line_parser`类来分析 (而不是`parse_command_line`) ，并且调用该类的 allow_unregistered 方法：

```cpp
parsed_options = 
    command_line_parser(argv, argc).
    options(desc).allow_unregistered().run();
```

如果使用配置文件，则在调用`parse_config_file`的时候第三个参数传递true:

```cpp 
parse_config_file<char>(cfgfilename, desc, true)
```
<!--more-->