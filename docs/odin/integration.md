---
layout: default
title: Ecosystem Integration
permalink: /docs/odin/integration/
---

[Back to Odin docs](/docs/odin/)

# Ecosystem Integration

Odin is fully standalone. Ecosystem integration is a bonus, not a requirement.
Each tool works independently; shared configuration is a convenience.

## Odin + Heimdall (Shared Config)

Odin imports [Heimdall](/docs/heimdall/) directly as a Go library to read
shared configuration on startup:

| Key                   | Purpose                                    |
| --------------------- | ------------------------------------------ |
| `odin.database_path`  | Override default DB location               |
| `odin.backup_path`    | Override backup directory                  |
| `odin.theme`          | UI theme preference                        |
| `odin.auto_backup`    | Enable automatic backups                   |

If Heimdall's DB doesn't exist, Odin falls back to its own `settings` table
and platform defaults. Heimdall is never a hard dependency.

Odin also reads from other namespaces when integrating:

- `muninn.vault_path` — where Muninn's notes live (for the Knowledge section)
- `huginn.output_dir` — where to send PDF output

## Odin + Muninn (Knowledge Base)

When the `muninn` binary is on `PATH`, Odin enables a Knowledge section in the
sidebar. Odin shells out to the Muninn CLI:

| Operation   | Command                                          |
| ----------- | ------------------------------------------------ |
| List notes  | `muninn note list`                               |
| Search      | `muninn search "<query>"`                        |
| Create note | `muninn note new "<title>" --project odin`       |

Muninn is a flat-file knowledge base — notes are plain markdown files with text
search. There is no database or API server to connect to. CLI invocation is the
integration path.

If Muninn is not installed, the Knowledge section is hidden. No error, no
degradation — Odin works fine without it.

## Odin + Huginn (PDF Generation)

When Huginn is available, Odin enables PDF export for compliance reports:

1. Odin's report engine assembles data in Huginn's input format (markdown
   tables + YAML frontmatter)
2. Sends to Huginn for rendering
3. Presents a native save dialog for the output PDF

**OSHA forms** (300/300A/301) have specific layouts handled by Huginn's
compliance components.

If Huginn is not available, PDF buttons are disabled with an explanation. CSV
export is always available as a fallback.

## Discovery

Odin detects ecosystem tools on startup:

1. Check `PATH` for `muninn` and `huginn` binaries
2. Read Heimdall config for custom paths
3. Enable/disable UI sections based on availability

No daemon, no service registry, no socket discovery. Just binary presence on
`PATH` and shared config via Heimdall.

## Protocol

All inter-tool communication is via CLI invocation (stdout/stderr). No sockets,
no IPC, no running services required. This keeps the integration simple and
debuggable — you can run the same commands Odin runs from your terminal.

Future optimization: if a tool exposes a local HTTP API, Odin can prefer that
over CLI shelling. But CLI is the baseline contract.
