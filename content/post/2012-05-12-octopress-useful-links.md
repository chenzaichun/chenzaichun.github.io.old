+++
title = "octopress常用链接整理"
date = "2012-05-12T11:54:00+08:00"
comments = true
categories = ["tools"]
tags = ["octopress", "无所事事"]
status = "publish"
description = ""
+++


1. [Heroku上搭建octopress自动deploy环境](http://jasongarber.com/blog/2012/01/10/deploying-octopress-to-heroku-with-a-custom-buildpack) -- 这篇文章可以让你绕过生成静态页面的过程，通过heroku的build pack在push的时候自动将markdown编译成静态页面。

2. [Octopress博客分类添加中文支持](http://geron.heroku.com/blog/2012/03/octo-cate-cn-spo/)

3. [如何從Wordpress Migrate到Octopress](http://blog.xdite.net/posts/2011/10/09/how-to-migrate-blog-from-wordpress-to-octopress/)

4. [301轉址](http://blog.xdite.net/posts/2011/10/09/how-to-migrate-blog-from-wordpress-to-octopress/) -- 重定向feed到atom.xml

    比如我的设置:

    <!--more-->

```ruby
use Rack::Rewrite do
 r301 %r{/\?feed=(.*)?}, "{{ site.url }}/atom.xml"
 r301 %r{/feed}, "{{ site.url }}/atom.xml"
end
```

5. [添加 related post](http://imwuyu.me/blog/configuring-octopress.html/)

6. [Plugin for Octopress to generate tag pages](https://github.com/robbyedwards/octopress-tag-pages)

7. [A tag cloud plugin for Octopress](https://github.com/robbyedwards/octopress-tag-cloud)

