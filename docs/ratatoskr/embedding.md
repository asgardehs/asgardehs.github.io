---
layout: docs
title: Embedding Python Libraries
permalink: /docs/ratatoskr/embedding/
---

[Back to Ratatoskr docs](/docs/ratatoskr/)

# Embedding Python Libraries

Ratatoskr can bundle pip packages alongside the embedded interpreter, so the
Python environment your Go binary ships is self-contained. The workflow uses
`go generate` to produce platform-specific package archives at build time and
`embed_util` to load them at runtime.

## Generator

Create a generator under your application (for example,
`internal/pylibs/generate/main.go`):

```go
package main

import "github.com/asgardehs/ratatoskr/pip"

func main() {
    err := pip.CreateEmbeddedPipPackagesForKnownPlatforms(
        "requirements.txt",
        "./data/",
    )
    if err != nil {
        panic(err)
    }
}
```

## `go:generate` Directive

Add a `go:generate` directive in a sibling file (`internal/pylibs/gen.go`):

```go
package pylibs

//go:generate go run -tags ratatoskr_embed ./generate
```

## Requirements File

Supply a `requirements.txt` alongside the generator:

```
jinja2==3.1.2
```

## Generating the Archives

Run:

```bash
go generate -tags ratatoskr_embed ./...
```

This populates `internal/pylibs/data` with the platform-specific package
archives.

The `ratatoskr_embed` build tag is required because the generator imports
Ratatoskr's embedded Python distribution, which is gated behind that tag;
without it the generator will fail to launch Python.

## Loading at Runtime

Load the generated archives at runtime via `embed_util.NewEmbeddedFiles()`
and attach the extracted path to your `EmbeddedPython` instance with
`AddPythonPath`. This is the same pattern the upstream
[go-jinja2](https://github.com/kluctl/go-jinja2) project uses.
