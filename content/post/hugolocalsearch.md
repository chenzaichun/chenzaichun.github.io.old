+++
title = "Hugo Local Search"
date = 2017-11-26
lastmod = 2017-11-26T18:02:32+08:00
tags = ["hugo", "org"]
categories = ["emacs"]
draft = false
+++

## Hugo本地搜索 {#hugo本地搜索}

-   官方的search选项见这里: <https://gohugo.io/tools/search/>
-   其他选项: <https://blog.jimmycai.com/p/hugo-local-search/>
-   还有这篇日文: <http://rs.luminousspice.com/hugo-site-search/>
-   比较靠谱的这里: <https://keyin.me/post/hugo-guidance/>

<!--more-->


### 配置 {#配置}

修改 `config.toml`,

```toml
[outputs]
  home = ["HTML", "RSS", "JSON"]
```

修改 `theme` 下 `partials/index.json`

```json
/// YOUR_TEMPLATE/partials/index.json
[
    {{ range $index, $element := (where .Site.RegularPages "Type" "post") }}
        {
            "title" : {{ jsonify .Title }},
            "date" : {{- jsonify .Date.Format ( or .Site.Params.dateFormat "2006, Jan 02" ) -}},
            "url" : {{ jsonify .Permalink }},
            "content": {{ jsonify .Summary }},
            "tag" : {{ jsonify .Params.Tag }}
        }{{if not (eq $index (sub (len (where .Site.RegularPages "Type" "post")) 1 )) }} , {{end}}
    {{ end }}
]
```

`YOUR_TEMPLATE/partials/_default/search.html`

```html
<main id="searchLayout" class="container">
    <input id="search-input" type="text" placeholder="Type something and hit enter"/>
    <p id="search-info"></p>
    <div id="search-result"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fuse.js/3.0.5/fuse.min.js"></script>
    <script src="search.js"></script>
</main>
```
