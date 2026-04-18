---
layout: docs
title: Heimdall Go API
permalink: /docs/heimdall/api/
---

[Back to Heimdall docs](/docs/heimdall/)

# Go API

Import path: `github.com/asgardehs/heimdall`

## Opening the Store

```go
// Use platform default database path
h, err := heimdall.Open(logger)

// Use a specific database path
h, err := heimdall.OpenPath("/path/to/heimdall.db", logger)

// With a change notifier
h, err := heimdall.Open(logger, heimdall.WithNotifier(myNotifier))

defer h.Close()
```

On open, Heimdall creates the database if it doesn't exist, runs migrations,
and seeds default schemas and values.

---

## Core Methods

### Get

```go
func (h *Heimdall) Get(namespace, key string) (*ConfigEntry, error)
```

Retrieves a single config entry. Returns `ConfigError` with code
`ErrConfigNotFound` if the key doesn't exist.

### Set

```go
func (h *Heimdall) Set(namespace, key, value string) error
```

Writes a config value. If a schema exists for the key, the value is validated
against it. Records a history entry and notifies the change notifier.

Returns `ConfigError` with code `ErrConfigValidation` if validation fails.

### List

```go
func (h *Heimdall) List(namespace string) ([]ConfigEntry, error)
```

Returns all config entries for a namespace.

### Reset

```go
func (h *Heimdall) Reset(namespace, key string) error
```

Resets a config key to its schema default. Returns an error if no schema
default exists for the key.

### Schema

```go
func (h *Heimdall) Schema(namespace string) ([]ConfigSchema, error)
```

Returns all schema entries for a namespace, describing available keys, types,
defaults, and whether they're required.

### Close

```go
func (h *Heimdall) Close() error
```

Closes the underlying database connection.

---

## Types

### ConfigEntry

```go
type ConfigEntry struct {
    Namespace string
    Key       string
    Value     string
    Type      string   // string, path, secret, number, boolean, array, enum
    Source    string   // "default" or "user"
    UpdatedAt string
    UpdatedBy string
}
```

### ConfigSchema

```go
type ConfigSchema struct {
    Namespace   string
    Key         string
    Type        string
    Description string
    DefaultVal  string
    Required    bool
    Choices     []string  // for enum type
}
```

### ConfigError

```go
type ConfigError struct {
    Code    int
    Message string
}
```

Error codes:

| Code     | Constant              | Meaning                    |
| -------- | --------------------- | -------------------------- |
| `-32002` | `ErrConfigNotFound`   | Key does not exist         |
| `-32003` | `ErrConfigValidation` | Value fails schema check   |
| `-32004` | `ErrPermissionDenied` | Operation not allowed      |
| `-32602` | `ErrInvalidParams`    | Bad input parameters       |

---

## Change Notification

```go
type ChangeNotifier interface {
    NotifyChange(namespace, key, value, oldValue string)
}
```

Called on every `Set` and `Reset`. Register with `WithNotifier`:

```go
type myNotifier struct{}

func (n *myNotifier) NotifyChange(ns, key, val, old string) {
    fmt.Printf("config changed: %s.%s = %s (was %s)\n", ns, key, val, old)
}

h, err := heimdall.Open(logger, heimdall.WithNotifier(&myNotifier{}))
```

---

## Validation

Heimdall validates values against their schema type:

| Type      | Accepts                                        |
| --------- | ---------------------------------------------- |
| `string`  | Any text                                       |
| `path`    | Any text (semantic hint only)                  |
| `secret`  | Any text (masked in CLI display)               |
| `number`  | Must parse as float64                          |
| `boolean` | `"true"` or `"false"` only                     |
| `array`   | Must be valid JSON array                       |
| `enum`    | Must be in the schema's `Choices` list         |

Validation only applies when a schema exists for the key. Keys without schemas
accept any value.
