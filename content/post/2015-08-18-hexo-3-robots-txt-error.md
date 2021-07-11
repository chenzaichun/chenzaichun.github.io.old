+++
title = "升级到 Hexo 3后发现 robots.txt失效"
tags = ["programming", "hexo"]
categories = ["programming"]
date = "2015-08-18T13:30:00+08:00"
description = ""
+++


升级到 hexo3之后一直没注意，今天查看`Google WebMaster Tools`发现竟然没抓去多少，查看原来的`robots.txt`, 是这样的:

```
---
layout: nil
---
User-agent: *
Disallow: 
Sitemap: {{ site.url }}/sitemap.xml 
```

打开生成的[robots.txt](/robots.txt)查看，竟然没有解析, 竟然是这样的

<!-- more -->

```
---
layout: nil
---
User-agent: *
Disallow: 
Sitemap: {{ site.url }}/sitemap.xml
```

完全没有处理这个文件, 安装`hexo-generator-robotstxt`, 由于官方的版本只有一个 sitemap，如果要添加 baidusitemap 的话，可以使用我的 fork [hexo-generator-robotstxt](https://github.com/chenzaichun/hexo-generator-robotstxt)

```sh
npm install chenzaichun/hexo-generator-robotstxt --save
```

修改`_config.yml`文件，加入

```
plugins:
- hexo-generator-robotstxt

robotstxt:
  useragent: "*"
  sitemap:
    - /sitemap.xml
    - /baidusitemap.xml
```


{%ruby 搞定|无聊 %}




<!--more-->