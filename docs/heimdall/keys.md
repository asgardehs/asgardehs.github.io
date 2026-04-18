---
layout: docs
title: Configuration Keys
permalink: /docs/heimdall/keys/
---

[Back to Heimdall docs](/docs/heimdall/)

# Configuration Keys

Heimdall organizes config into **namespaces** — one per tool, plus a shared
`ai` namespace.

## odin

Desktop app configuration.

| Key            | Type    | Default    | Description                |
| -------------- | ------- | ---------- | -------------------------- |
| `database_path`| path    | _(empty)_  | Path to Odin SQLite DB     |
| `backup_path`  | path    | _(empty)_  | Path for database backups  |
| `theme`        | string  | `"system"` | UI theme                   |
| `auto_backup`  | boolean | `"true"`   | Enable automatic backups   |

## muninn

Knowledge base configuration.

| Key            | Type   | Default     | Description                |
| -------------- | ------ | ----------- | -------------------------- |
| `vault_path`   | path   | _(empty)_   | Path to note vault         |

## huginn

AI/analysis component configuration.

| Key            | Type   | Default    | Description                     |
| -------------- | ------ | ---------- | ------------------------------- |
| `default_theme`| string | `"default"`| Default rendering theme         |
| `output_dir`   | path   | _(empty)_  | Output directory for renders    |

## ai

Cross-cutting AI configuration. Disabled by default — opt-in per user.

| Key             | Type    | Default   | Description                        |
| --------------- | ------- | --------- | ---------------------------------- |
| `enabled`       | boolean | `"false"` | Master AI enable/disable switch    |
| `provider`      | enum    | _(empty)_ | AI provider: `anthropic`, `openai` |
| `api_key`       | secret  | _(empty)_ | API key (masked in CLI output)     |
| `odin_access`   | array   | `"[]"`    | Modules AI can access in Odin      |
| `muninn_access` | array   | `"[]"`    | Features AI can access in Muninn   |
| `huginn_access` | array   | `"[]"`    | Features AI can access in Huginn   |

---

## Schema Types

| Type      | Validation                                     | Display     |
| --------- | ---------------------------------------------- | ----------- |
| `string`  | Any text                                       | Plaintext   |
| `path`    | Any text (semantic hint for file paths)        | Plaintext   |
| `secret`  | Any text                                       | Masked      |
| `number`  | Must parse as float64                          | Plaintext   |
| `boolean` | `"true"` or `"false"` only                     | Plaintext   |
| `array`   | Must be valid JSON array                       | Plaintext   |
| `enum`    | Must be in the schema's allowed choices        | Plaintext   |

---

## Change History

Every `Set` and `Reset` operation records a history entry in the
`config_history` table with the old value, new value, who changed it, and when.
This is useful for auditing and debugging configuration drift.
