
[![GoDoc](https://godoc.org/github.com/tj/go-config?status.svg)](http://godoc.org/github.com/tj/go-config)
[![Build Status](https://travis-ci.org/tj/go-config.svg?branch=master)](https://travis-ci.org/tj/go-config)

# go-config

 Simpler Go configuration with structs.

## Features

- Declare configuration with structs and tags
- Type coercion out of the box
- Validation out of the box
- Pluggable resolvers
- Built-in resolvers (flag, env)
- Unambiguous resolution (must be specified via `from`)

## Example

Source:

```go
package main

import (
  "log"
  "os"
  "time"

  "github.com/tj/go-config"
)

type Options struct {
  Timeout     time.Duration `desc:"message timeout"`
  Concurrency uint          `desc:"message concurrency"`
  CacheSize   config.Bytes  `desc:"cache size in bytes"`
  BatchSize   uint          `desc:"batch size" validate:"min=1,max=1000"`
  LogLevel    string        `desc:"log severity level" from:"env,flag"`
}

var options = &Options{
  Timeout:     time.Second * 5,
  Concurrency: 10,
  CacheSize:   config.ParseBytes("100mb"),
  BatchSize:   250,
}

func main() {
  config.MustResolve(options)
  log.Printf("%+v", options)
}
```

Command-line:

```
$ LOG_LEVEL=error example --timeout 10s --concurrenct 100 --cache-size 1gb
```

# License

MIT