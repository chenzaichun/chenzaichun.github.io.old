+++
categories = ["emacs", "Linux", "org2blog", "tools"]
comments = true
published = true
status = "publish"
date = "2011-11-02T11:11:11+08:00"
tags = ["emacs", "Linux", "org2blog"]
title = "禁用zsh中的git prompt"
type = "post"
description = ""
+++

git prompt很多时候其实挺方面的，但是的我cd到一个nfs目录的时候，会出现卡很久的情况。果断disable掉。

首先根据你的theme找到$PROMPT 

```sh
echo $PROMPT
```

比如我的是: 

``` 
%{$fg_bold[red]%}➜ %{$fg_bold[green]%}%p %{$fg[cyan]%}%c %{$fg_bold[blue]%}$(git_prompt_info)%{$fg_bold[blue]%} % %{$reset_color%}
```

去掉`git_prompt_info`即可: 

```sh
export PROMPT="%{$fg_bold[red]%}➜ %{$fg_bold[green]%}%p %{$fg[cyan]%}%c %{$fg_bold[blue]%}%{$fg_bold[blue]%} % %{$reset_color%}"
```
<!--more-->