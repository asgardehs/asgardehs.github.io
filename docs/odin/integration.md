---
layout: docs
title: Ecosystem Integration
permalink: /docs/odin/integration/
---

[Back to Odin docs](/docs/odin/)

# Ecosystem Integration

Odin is fully standalone. **Heimdall is optional** — Odin runs the same with
or without it. When Heimdall is present, Odin reads shared config from it;
when it's not, Odin uses its own `settings` table and platform defaults.

## Odin + Heimdall (Shared Config)

Odin imports [Heimdall](/docs/heimdall/) directly as a Go library to read
shared configuration on startup:

| Key                   | Purpose                                    |
| --------------------- | ------------------------------------------ |
| `odin.database_path`  | Override default DB location               |
| `odin.backup_path`    | Override backup directory                  |
| `odin.theme`          | UI theme preference                        |
| `odin.auto_backup`    | Enable automatic backups                   |

If Heimdall's database doesn't exist or can't be opened, Odin falls back to
its own `settings` table and platform defaults. There is no hard dependency,
no warning at startup, and no degraded mode — Odin simply manages its own
configuration locally.

## Discovery

On startup, Odin attempts to open the Heimdall database at the platform
default location (or wherever `HEIMDALL_DATA_DIR` points). If the open
succeeds, Odin reads its `odin.*` keys. If it fails, Odin uses internal
defaults.

No daemon, no service registry, no socket discovery. Heimdall is a Go
library that operates on a local SQLite file — its presence is a
file-existence check, nothing more.
