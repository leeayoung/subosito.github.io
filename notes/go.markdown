---
title: Notes about Go (golang)
layout: page
permalink: /notes/go/
---

[Go](http://golang.org/) is expressive, consise, clean and efficient programming language. It has native concurrency mechanism and could compile very fast.

## Constructing query string

You can use `net/url` to construct query string for http request.

```go
package main

import (
    "net/url"
    "fmt"
)

func main() {
    baseUrl := "http://example.com/"
    params  := make(url.Values) // or, params := url.Values{}
    params.Add("type", "images")
    params.Add("q", "Golang!")

    finalUrl := fmt.Sprintf("%s?%s", baseUrl, params.Encode())
    fmt.Println(finalUrl)
    // http://example.com/?q=Golang%21&type=images
}
```

## Executable

```
$ pwd
/home/subosito/code/gocode/src/github.com/subosito/nexmo

$ tree
.
|-- nexmo-cli.go
`-- nexmo.go

$ go build
can't load package: package github.com/subosito/nexmo: found packages main (nexmo-cli.go) and nexmo (nexmo.go) in /home/subosito/code/gocode/src/github.com/subosito/nexmo

$ mkdir nexmo
$ mv nexmo-cli.go nexmo/main.go

$ tree
.
|-- nexmo
|   `-- main.go
`-- nexmo.go

$ tree $GOPATH/bin
/home/subosito/code/gocode/bin
|-- nexmo
`-- twilio
```

Now you can run the executable with `nexmo`. Ensure you have added `bin` directory to `PATH` variable:

```
$ export PATH=$GOPATH/bin:$PATH
```

## Auto-formatting Go code on Vim

For auto-formatting Go on Vim, you can use [vim-golang](https://github.com/jnwhiteh/vim-golang) plugin. This plugin comes with a function called :Fmt that reformats the source code using `gofmt`.

Then to perform auto-formatting when saving file, you can add it to `.vimrc`

```vim
autocmd FileType go autocmd BufWritePre <buffer> Fmt
```

