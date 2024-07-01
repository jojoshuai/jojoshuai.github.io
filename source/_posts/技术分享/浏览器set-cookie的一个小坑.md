---
title: 浏览器set-cookie的一个小坑
date: 2024-06-23 17:18:17
categories:
  - 技术分享
tags:
  - cookie
  - gin
---

背景：
在浏览器接收到 http 的 response 中，如果带有 set-

```code=go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.New()
	cookieKey := "test_cookie"
	router.GET("/set_cookie_by_no_domain", func(ctx *gin.Context) {
		ctx.SetCookie(cookieKey, "set_cookie_by_no_domain", 3600, "/", "", false, false)
		ctx.String(http.StatusOK, "set cookie: %s=%s", cookieKey, "set_cookie_by_no_domain")
	})
	router.GET("set_cookie_by_domain", func(ctx *gin.Context) {
		ctx.SetCookie(cookieKey, "set_cookie_by_domain", 3600, "/", "a.b.com", false, false)
		ctx.String(http.StatusOK, "set cookie: %s=%s", cookieKey, "set_cookie_by_domain")
	})
	router.GET("/valid_cookie", func(ctx *gin.Context) {
		cookieValue, err := ctx.Cookie(cookieKey)
		if err != nil {
			ctx.String(http.StatusOK, "no cookie")
			return
		}
		ctx.String(http.StatusOK, "cookie pass: %s=%s", cookieKey, cookieValue)
	})

	_ = router.Run(":8765")
}
```

1. 前置：本地设置 host`127.0.0.1 a.b.com`

2. 执行上述代码`go run main.go`

3. chrome 浏览器分别请求：

   1. `http://a.b.com:8765/set_cookie_by_no_domain`； HttpResponse Header 中`Set-Cookie:
test_cookie=set_cookie_by_no_domain; Path=/; Max-Age=3600`
   2. `http://a.b.com:8765/set_cookie_by_domain`
      HttpResponse Header 中`Set-Cookie:
test_cookie=set_cookie_by_domain; Path=/; Domain=a.b.com; Max-Age=3600`

4. 查看 cookie
   - test_cookie set_cookie_by_domain .a.b.com
   - test_cookie set_cookie_by_no_domain a.b.com
     发现两个 domain 是不一致的
5. 请求`http://a.b.com:8765/valid_cookie`，

   - request Header 中：`Cookie:
test_cookie=set_cookie_by_no_domain; test_cookie=set_cookie_by_domain`;cookie 会传入两个指

6. gin 读取只会读取一个
