+++
title = "Octopress添加robots.txt"
date = "2012-05-12T21:05:00+08:00"
comments = true
categories = ["tools"]
tags = ["octopress", "无所事事"]
description = ""
+++


添加`source/robots.txt`:

```
---
layout: nil
---
User-agent: *
Disallow: 
Sitemap: {{ site.url }}/sitemap.xml 
```
<!--more-->