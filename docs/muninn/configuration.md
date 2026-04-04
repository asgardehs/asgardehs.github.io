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

| Variable            | Default                          | Description                         |
| ------------------- | -------------------------------- | ----------------------------------- |
| `MUNINN_VAULT_PATH` | Platform default (see below)     | Vault directory location            |
| `MUNINN_DB_PATH`    | `<vault>/muninn.db`              | Database file location              |
| `MUNINN_MODEL_NAME` | `nomic-ai/nomic-embed-text-v1.5` | Embedding model to use              |
| `MUNINN_MODEL_DIR`  | Platform default (see below)     | Local cache for the embedding model |

### Examples

Use a custom vault location:

```bash
export MUNINN_VAULT_PATH="$HOME/knowledge"
```

Put the database somewhere else (useful if your vault is on a slow filesystem):

```bash
export MUNINN_DB_PATH="$HOME/.local/share/muninn/muninn.db"
```

---

## Data Locations

Muninn follows platform conventions for where it stores data.

### Vault directory

Where notes, the database, and the `.gitignore` live.

| Platform | Default path                            |
| -------- | --------------------------------------- |
| Linux    | `~/.local/share/muninn/`                |
| macOS    | `~/Library/Application Support/muninn/` |
| Windows  | `%LOCALAPPDATA%\muninn\`                |

### Model cache

Where the downloaded embedding model is stored (~130 MB).

| Platform | Default path                          |
| -------- | ------------------------------------- |
| Linux    | `~/.cache/muninn/models/`             |
| macOS    | `~/Library/Caches/muninn/models/`     |
| Windows  | `%LOCALAPPDATA%\muninn\cache\models\` |

---

## Vault Structure

After running `muninn init`, the vault directory looks like this:

```
muninn/
├── muninn.db          # snippets, note index, embeddings, wikilinks
├── notes/             # markdown vault
│   ├── some-topic.md
│   └── ...
└── .gitignore         # ignores muninn.db and model cache
```

The database is gitignored — it's fully regenerable from the markdown files (for
notes) and is the primary store for snippets. Run `git init` inside the vault
directory to start versioning your notes.

---

## ONNX Runtime

Muninn requires the ONNX Runtime shared library for local embedding inference.
If it's not installed, Muninn will attempt to download it automatically, but
installing it system-wide is recommended for reliability.

| Platform         | Install                                                                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Arch Linux       | `sudo pacman -S onnxruntime` or AUR `onnxruntime-bin`                                                                                 |
| Ubuntu/Debian    | Download from [ONNX Runtime releases](https://github.com/microsoft/onnxruntime/releases) and place `libonnxruntime.so` in `/usr/lib/` |
| macOS (Homebrew) | `brew install onnxruntime`                                                                                                            |
| Windows          | Download from ONNX Runtime releases and add the DLL directory to `PATH`                                                               |

Muninn searches standard library paths automatically. If installed to a
non-standard location, the library just needs to be on the system library path
(`LD_LIBRARY_PATH` on Linux, `DYLD_LIBRARY_PATH` on macOS).
