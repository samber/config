# Config
[![Documentation](https://godoc.org/github.com/samber/config?status.svg)](http://godoc.org/github.com/samber/config)
[![Mentioned in Awesome Go](https://awesome.re/mentioned-badge-flat.svg)](https://github.com/avelino/awesome-go)
[![Build Status](https://travis-ci.org/samber/config.svg?branch=master)](https://travis-ci.org/samber/config)
[![Go Report Card](https://goreportcard.com/badge/github.com/samber/config)](https://goreportcard.com/report/github.com/samber/config)
[![Coverage Status](https://coveralls.io/repos/github/samber/config/badge.svg?branch=master)](https://coveralls.io/github/samber/config?branch=master)
[![GitHub issues](https://img.shields.io/github/issues/samber/config.svg)](https://github.com/samber/config/issues)
[![license](https://img.shields.io/github/license/samber/config.svg?maxAge=2592000)](https://github.com/samber/config/LICENSE)
[![Release](https://img.shields.io/github/release/samber/config.svg?label=Release)](https://github.com/samber/config/releases)

**Samber EDIT => Removed PARENT__CHILD nested environment variables**

Manage your application config as a typesafe struct in as little as two function calls.

```go
type MyConfig struct {
	DatabaseUrl string `config:"DATABASE_URL"`
	FeatureFlag bool   `config:"FEATURE_FLAG"`
	Port        int // tags are optional. PORT is assumed
	...
}

var c MyConfig
config.FromEnv().To(&c)
```

## How It Works

Its just simple, pure stdlib.

* A field's type determines what [strconv](https://golang.org/pkg/strconv/) function is called.
* All string conversion rules are as defined in the [strconv](https://golang.org/pkg/strconv/) package
* If chaining multiple data sources, data sets are merged.
  Later values override previous values.
  ```go
  config.From("dev.config").FromEnv().To(&c)
  ```

* Unset values remain as their native [zero value](https://tour.golang.org/basics/12)
* Env vars map to struct fields case insensitively
    * NOTE: Also true when using struct tags.

## Why you should use this

* Its the cloud-native way to manage config. See [12 Factor Apps](https://12factor.net/config)
* Simple:
    * only 2 lines to configure.
* Composeable:
    * Merge local files and environment variables for effortless local development.
* small:
    * only stdlib
    * < 180 LoC

## Design Philosophy

Opinionated and narrow in scope. This library is only meant to do config binding.
Feel free to use it on its own, or alongside other libraries.

* Only structs at the entry point. This keeps the API surface small.

* Slices are space delimited. This matches how environment variables and commandline args are handled by the `go` cmd.

* No slices of structs. The extra complexity isn't warranted for such a niche usecase.

* No maps. The only feature of maps not handled by structs for this usecase is dynamic keys.

* No pointer members. If you really need one, just take the address of parts of your struct.
