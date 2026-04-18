---
layout: docs
title: Heimdall Quick Start
permalink: /docs/heimdall/quickstart/
---

[Back to Heimdall docs](/docs/heimdall/)

# Quick Start

## As a Go Library

Add Heimdall to your module:

```bash
go get github.com/asgardehs/heimdall
```

Open the config store and start reading/writing:

```go
package main

import (
    "log/slog"
    "fmt"

    "github.com/asgardehs/heimdall"
)

func main() {
    logger := slog.Default()

    h, err := heimdall.Open(logger)
    if err != nil {
        panic(err)
    }
    defer h.Close()

    // Read a config value
    entry, err := h.Get("muninn", "vault_path")
    if err != nil {
        panic(err)
    }
    fmt.Println(entry.Value)

    // Write a config value (validated against schema)
    if err := h.Set("ai", "enabled", "true"); err != nil {
        panic(err)
    }
}
```

On first use, Heimdall creates the database at the platform default location
and seeds it with default schemas and values.

## As a CLI

Build the CLI:

```bash
git clone https://github.com/asgardehs/heimdall.git
cd heimdall
go build -o heimdall ./cmd/heimdall
```

Then manage config from the terminal:

```bash
# See all config for a namespace
heimdall config list odin

# Get a specific key
heimdall config get ai provider

# Set a value (validated against schema)
heimdall config set ai provider anthropic

# Reset to default
heimdall config reset odin theme
```

## Database Location

Heimdall stores config in a local SQLite database. The default path follows
platform conventions:

| Platform | Default path                                    |
| -------- | ----------------------------------------------- |
| Linux    | `~/.local/share/heimdall/heimdall.db`           |
| macOS    | `~/Library/Application Support/heimdall/heimdall.db` |
| Windows  | `%APPDATA%\heimdall\heimdall.db`                |

Override with the `HEIMDALL_DATA_DIR` environment variable.

No CGO required — Heimdall uses pure Go SQLite (`modernc.org/sqlite`).
