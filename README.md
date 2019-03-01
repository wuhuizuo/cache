# Cache gin's middleware

[![Build Status](https://travis-ci.org/wuhuizuo/cache.svg)](https://travis-ci.org/wuhuizuo/cache)
[![codecov](https://codecov.io/gh/wuhuizuo/cache/branch/master/graph/badge.svg)](https://codecov.io/gh/wuhuizuo/cache)
[![Go Report Card](https://goreportcard.com/badge/github.com/wuhuizuo/cache)](https://goreportcard.com/report/github.com/wuhuizuo/cache)
[![GoDoc](https://godoc.org/github.com/wuhuizuo/cache?status.svg)](https://godoc.org/github.com/wuhuizuo/cache)

Gin middleware/handler to enable Cache.

## Usage

### Start using it

Download and install it:

```sh
$ go get github.com/wuhuizuo/cache
```

Import it in your code:

```go
import "github.com/wuhuizuo/cache"
```

### Canonical example:

See the [example](example/example.go)

```go
package main

import (
	"fmt"
	"time"

	"github.com/wuhuizuo/cache"
	"github.com/wuhuizuo/cache/persistence"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	store := persistence.NewInMemoryStore(time.Second)
	
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	})
	// Cached Page
	r.GET("/cache_ping", cache.CachePage(store, time.Minute, func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	}))

	// Listen and Server in 0.0.0.0:8080
	r.Run(":8080")
}
```
