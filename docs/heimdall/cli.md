---
layout: docs
title: Heimdall CLI Reference
permalink: /docs/heimdall/cli/
---

[Back to Heimdall docs](/docs/heimdall/)

# CLI Reference

The Heimdall CLI provides direct access to the config store from the terminal.
All commands are under the `config` subcommand.

## config get

Retrieve a single config value.

```
heimdall config get <namespace> <key>
```

### Example

```bash
$ heimdall config get odin database_path
odin.database_path = /home/you/.local/share/odin/odin.db (source: default, type: path)
```

Secrets are masked:

```bash
$ heimdall config get ai api_key
ai.api_key = sk-a...xyz9 (source: user, type: secret)
```

---

## config set

Write a config value. Validates against the schema if one exists.

```
heimdall config set <namespace> <key> <value>
```

### Examples

```bash
$ heimdall config set ai provider anthropic
ai.provider = anthropic

$ heimdall config set ai enabled true
ai.enabled = true

$ heimdall config set ai provider invalid
Error: validation failed: value "invalid" not in allowed choices [anthropic openai]
```

---

## config list

List all config entries for a namespace.

```
heimdall config list <namespace>
```

### Example

```bash
$ heimdall config list ai
  enabled     = false       (default)
  provider    = anthropic   (user)
  api_key     = sk-a...xyz9 (user)
  odin_access = []          (default)
```

---

## config reset

Reset a config key to its schema default.

```
heimdall config reset <namespace> <key>
```

### Example

```bash
$ heimdall config reset odin theme
odin.theme reset to default.
```

Returns an error if no schema default exists for the key.
