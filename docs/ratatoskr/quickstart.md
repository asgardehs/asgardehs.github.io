---
layout: docs
title: Ratatoskr Quick Start
permalink: /docs/ratatoskr/quickstart/
---

[Back to Ratatoskr docs](/docs/ratatoskr/)

# Quick Start

## Installation

```bash
go get github.com/asgardehs/ratatoskr
```

Requires Go 1.26 or later. No CGO — Ratatoskr uses subprocess IPC rather
than Python bindings, so cross-compilation is straightforward.

## Building from Source

```bash
git clone https://github.com/asgardehs/ratatoskr.git
cd ratatoskr
go build ./...
```

## First Invocation

```go
package main

import (
    "os"

    "github.com/asgardehs/ratatoskr/python"
)

func main() {
    ep, err := python.NewEmbeddedPython("example")
    if err != nil {
        panic(err)
    }

    cmd, err := ep.PythonCmd("-c", "print('hello')")
    if err != nil {
        panic(err)
    }
    cmd.Stdout = os.Stdout
    cmd.Stderr = os.Stderr
    if err := cmd.Run(); err != nil {
        panic(err)
    }
}
```

`NewEmbeddedPython` extracts the embedded distribution to a temporary
directory on first use and skips the extraction on subsequent runs if the
previously extracted copy is intact.

## Persistent Extraction for Desktop Apps

`NewEmbeddedPython` extracts into the system temp directory (`/tmp` on Linux,
`%TEMP%` on Windows, `/var/folders/...` on macOS). That location may be wiped
on reboot or cleaned by `systemd-tmpfiles`, forcing re-extraction on every
launch. For long-lived desktop applications, use
`NewEmbeddedPythonInCacheDir` instead:

```go
ep, err := python.NewEmbeddedPythonInCacheDir("myapp")
```

This places the extracted distribution under the user's OS cache directory:

- **Linux** — `~/.cache/myapp/python-<hash>` (respecting `$XDG_CACHE_HOME`)
- **macOS** — `~/Library/Caches/myapp/...`
- **Windows** — `%LOCALAPPDATA%\myapp\...`

The extraction cost is paid once per installed version, and survives reboots.

## How It Works

Ratatoskr uses the standalone Python distributions published by
[astral-sh/python-build-standalone](https://github.com/astral-sh/python-build-standalone).
At build time, the release workflow downloads, extracts, and packages the
supported distributions, which are then embedded into the Go binary using
`//go:embed`.

At runtime, `NewEmbeddedPython` extracts the embedded distribution into a
temporary folder the first time it is called. Subsequent calls reuse the
extracted distribution after verifying its integrity, so the extraction cost
is paid once per install, not once per invocation. The `EmbeddedPython`
object then exposes the interpreter as a helper for constructing
`exec.Cmd`-style Python invocations.
