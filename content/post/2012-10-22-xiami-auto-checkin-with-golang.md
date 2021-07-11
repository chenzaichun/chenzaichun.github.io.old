+++
title = "虾米自动签到go语言版本"
date = "2012-10-22T14:12:00+08:00"
comments = true
categories = ["programming"]
tags = ["go", "golang"]
description = ""
+++


原来在网上找了一份python版实现, 见[这里](http://yesokay.herokuapp.com/xiami-checkin.html) ，仿照着这个用go语言实现了一编。

只是在实现的过程中，go的http cookie比较麻烦，也是从网上找了一段`InMemoryCookieJar`

总的来说go语言还是挺方便的。

直接贴代码吧：

<!--more-->

```go
package main

import (
	"fmt"
	"os"
	"net/http"
	"net/url"
	"io/ioutil"
	"regexp"
	"strings"
)

type InMemoryCookieJar struct{
	storage map[string][]http.Cookie
}

// buggy... but works

func (jar InMemoryCookieJar) SetCookies(u *url.URL, cookies []*http.Cookie) {
	for _, ck := range cookies {
		path := ck.Domain
		if ck.Path != "/" {
			path = ck.Domain + ck.Path
		}
		if ck.Domain[0] == '.' {
			path = path[1:]		// FIXME: .hi.baidu.com won't match hi.baidu.com
		}
		if _, found := jar.storage[path]; found {
			setted := false
			for i, v := range jar.storage[path] {
				if v.Name == ck.Name {
					jar.storage[path][i] = *ck
					setted = true
					break
				}
			}
			if ! setted {
				jar.storage[path] = append(jar.storage[path], *ck)
			}
		} else {
			jar.storage[path] = []http.Cookie{*ck}
		}
	}
}

func (jar InMemoryCookieJar) Cookies(u *url.URL) []*http.Cookie {
	cks := []*http.Cookie{}
	//fmt.Println("get cookies", u)
	for pattern, cookies := range jar.storage {
		if strings.Contains(u.String(), pattern) {
			for i := range cookies {
				cks = append(cks, &cookies[i])
				//fmt.Println("add cookie", cookies[i].Name, cookies[i].Value)
			}
		}
	}
	return cks
}

func newCookieJar() InMemoryCookieJar {
	storage := make(map[string][]http.Cookie)
	return InMemoryCookieJar{storage}
}

func checkError(err error) {
	if err != nil {
		fmt.Println("Fatal error ", err.Error())
		os.Exit(1)
	}
}

func check(b string)  {
	pattern := regexp.MustCompile(`<div class="idh">(已连续签到\d+天)</div>`)
	s := pattern.FindStringSubmatch(string(b))
	
	if len(s) > 0 {
		fmt.Println("[Succeed] Checkin Already!", s[1])
	} else {
		fmt.Println("[Error] Login Failed!")
	}
}

func main() {
	if len(os.Args) != 3 {
		fmt.Println("Usage: ", os.Args[0], "email password")
		os.Exit(1)
	}

	email := os.Args[1]
	passwd := os.Args[2]

	client := http.Client{Jar: newCookieJar()}

	resp, err := client.PostForm("http://www.xiami.com/web/login", url.Values{
		"email": {email},
		"password": {passwd},
		"LoginButton": {"登陆"},
	})
	checkError(err)
	resp.Body.Close()
	
	resp, err = client.Get("http://www.xiami.com/web/login")
	checkError(err)
	b, _ := ioutil.ReadAll(resp.Body)
	defer resp.Body.Close()

	// check if login already
	pattern := regexp.MustCompile(`<a class="check_in" href="(.*?)">`)
	if m := pattern.FindStringSubmatch(string(b)); len(m) == 0 {
		// already checkin | login failed
	        check(string(b))
	} else {
		// start checking in
		url := "http://www.xiami.com" + m[1]
		request, err := http.NewRequest("GET", url, nil)
		checkError(err)
		request.Header.Add("Referer", "http://www.xiami.com/web/login")
		request.Header.Add("User-Agent", "Opera/9.60")
		resp, err = client.Do(request)
		checkError(err)
		b, _ := ioutil.ReadAll(resp.Body)
		defer resp.Body.Close()
		check(string(b))
	}
}
```
