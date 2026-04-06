---
layout: default
title: Configuration
permalink: /docs/muninn/configuration/
---

[Back to Muninn docs](/docs/muninn/)

# Configuration

Muninn is configured entirely through environment variables — there is no config
file. All variables are optional; the defaults follow platform conventions.

## Environment Variables

| Variable            | Default                      | Description              |
| ------------------- | ---------------------------- | ------------------------ |
| `MUNINN_VAULT_PATH` | Platform default (see below) | Vault directory location |

### Examples

Use a custom vault location:

```bash
export MUNINN_VAULT_PATH="$HOME/knowledge"
```

---

## Data Locations

Muninn follows platform conventions for where it stores data.

### Vault directory

Where your notes live on disk.

| Platform | Default path                            |
| -------- | --------------------------------------- |
| Linux    | `~/.local/share/muninn/`                |
| macOS    | `~/Library/Application Support/muninn/` |
| Windows  | `%LOCALAPPDATA%\muninn\`                |

---

## Vault Structure

After running `muninn init`, the vault directory looks like this:

```
muninn/
└── notes/             # markdown vault
    ├── some-topic.md
    ├── journal/       # daily notes (created by daily note command)
    │   ├── 2026-04-W1-05.md
    │   └── ...
    └── ...
```

Notes are plain markdown files with optional YAML frontmatter. Run `git init`
inside the vault directory to start versioning your notes.

---

## VS Code Extension Settings

The Muninn VS Code extension adds these settings (configurable in VS Code's
Settings UI or `settings.json`):

| Setting                                | Type    | Default     | Description                                            |
| -------------------------------------- | ------- | ----------- | ------------------------------------------------------ |
| `muninn.binaryPath`                    | string  | `"muninn"`  | Path to the muninn binary                              |
| `muninn.vaultPath`                     | string  | `""`        | Override vault path (defaults to env or platform path)  |
| `muninn.dailyNotes.folder`             | string  | `"journal"` | Subdirectory for daily notes within the vault           |
| `muninn.codeLens.enabled`              | boolean | `true`      | Show reference count above note titles                  |
| `muninn.diagnostics.unresolvedLinks`   | boolean | `true`      | Show warnings for broken wikilinks                      |
| `muninn.semanticTokens.enabled`        | boolean | `true`      | Enable LSP-driven syntax highlighting                   |

These settings are passed to the LSP server on startup via initialization
options. Changes take effect after restarting the extension (reload window).
