+++
title = "修改ibus-googlepinyin候选词的个数"
date = "2012-11-21T16:17:00+08:00"
comments = true
categories = ["programming", "tools"]
tags = ["linux"]
description = ""
+++


默认情况下ibus-googlepinyin的默认候选词个数是5个，但是ibus-googlepinyin并没有提供设置选项的功能，同时也没有配置文件。研究ibus的代码发现原来是写死了的 :(

解决办法，修改`/usr/share/ibus-googlepinyin/engine.py`

将下面一行中的`set_page_size`参数替换为自己想要的，比如10个

```python
class Engine(ibus.EngineBase):

    def __init__(self, bus, object_path):
        super(Engine, self).__init__(bus, object_path)
        im_open_decoder()
        self.__is_invalidate = False
        self.__prepinyin_string = u""
        self.__lookup_table = ibus.LookupTable()
        self.__lookup_table.set_page_size(10)
```

重启ibus就行了。
<!--more-->