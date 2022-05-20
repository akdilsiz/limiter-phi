# Limiter Phi
[![CircleCI](https://circleci.com/gh/akdilsiz/limiterphi/tree/master.svg?style=svg)](https://circleci.com/gh/akdilsiz/limiterphi/tree/master)  
[![GitHub license](https://img.shields.io/github/license/akdilsiz/limiterphi)](https://github.com/akdilsiz/limiterphi/blob/master/LICENSE)
[![codecov](https://codecov.io/gh/akdilsiz/limiterphi/branch/master/graph/badge.svg)](https://codecov.io/gh/akdilsiz/limiterphi)
[![Go Report Card](https://goreportcard.com/badge/github.com/akdilsiz/limiterphi)](https://goreportcard.com/report/github.com/akdilsiz/limiterphi)\
phi middleware of **[ulule/limiter](https://github.com/ulule/limiter)** package.\
For detailed documentation [https://github.com/ulule/limiter](https://github.com/ulule/limiter)

## Example
```go
package main

import (
    "github.com/akdilsiz/limiterphi"
    "github.com/fate-lovely/phi"
    "github.com/ulule/limiter/v3"
    "github.com/ulule/limiter/v3/drivers/store/memory"
    "github.com/valyala/fasthttp"
    "log"
)

func main() {
    store := memory.NewStore()

    rate, err := limiter.NewRateFromFormatted("10-M")
    if err != nil {
        panic(err)
    } 
    middleware := limiterphi.NewMiddleware(limiter.New(store, rate))

    router := phi.NewRouter()
    router.Use(middleware.Handle)
    router.Get("/", func(ctx *fasthttp.RequestCtx) {
        ctx.SetStatusCode(fasthttp.StatusOK)
        ctx.SetContentType("application/json")
        ctx.SetBodyString(`{"message":"OK"}`)
    })

    log.Fatal(fasthttp.ListenAndServe(":3001", router.ServeFastHTTP))
}
```

## LICENSE
[MIT](https://github.com/akdilsiz/limiterphi/LICENSE) 
