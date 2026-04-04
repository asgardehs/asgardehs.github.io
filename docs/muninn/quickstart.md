---
layout: default
title: Quick Start
permalink: /docs/muninn/quickstart/
---

[Back to Muninn docs](/docs/muninn/)

# Quick Start

After [building](/docs/muninn/requirements/) and adding the binary to your `PATH`, run:

```bash
muninn init
```

This creates the vault directory, database, and downloads the embedding model
(~130 MB, one-time). Once complete, Muninn is ready to use from the terminal.

## Register with Your Tools

Muninn works through three interfaces — CLI, MCP server, and LSP — all backed by
the same database. The `install` command registers Muninn with the tools that
use them.

```bash
muninn install
```

With no flags, this registers with everything it can find. Use flags to target
specific tools:

### `--desktop` — Claude Desktop

```bash
muninn install --desktop
```

Writes a server entry to Claude Desktop's config file so it can launch
`muninn serve` automatically. After installing, restart Claude Desktop and
Muninn's tools will appear in the conversation — Claude can save snippets,
search your knowledge base, and create notes on your behalf mid-conversation.

### `--code` — Claude Code

```bash
muninn install --code
```

Same as above, but for Claude Code (the CLI/editor agent). Writes the entry to
`~/.claude.json`. This lets Claude Code access your knowledge base during
terminal sessions — useful for recalling snippets, referencing past decisions,
or saving context without leaving the terminal.

### `--vscode` — VS Code Extension

```bash
muninn install --vscode
```

Installs the `.vsix` extension that ships with Muninn. This launches the LSP
server in the background and gives you:

- `[[wikilink]]` completions and syntax highlighting in markdown
- Go to Definition on wikilinks (jump to the linked note)
- Backlink references (find every note that links to the current one)
- Hover previews of linked notes
- Diagnostics for broken links
- A "New Note" command (`Ctrl+Shift+N`)

The extension requires the Muninn binary on your `PATH` (or configured via the
`muninn.binaryPath` setting).

> **Note:** The `--vscode` flag looks for a pre-built `muninn-0.1.0.vsix` near
> the binary. If you built from source, make sure you've
> [built the extension](/docs/muninn/requirements/#vs-code-extension) first.

## First Steps

### Save a snippet

```bash
# From a file
muninn save --title "workspace IPC handler" --lang go --tags "hyprland,ipc" handler.go

# From stdin
echo "btrfs subvol set-default requires the subvol ID, not the path" | muninn save --tags btrfs -
```

### Find it later

```bash
muninn search "that btrfs subvolume permissions thing"
```

Semantic search means you don't need exact words — describe what you remember
and Muninn finds the closest match.

### Create a note

```bash
muninn note new "Btrfs Subvolume Gotchas"
```

Opens a new markdown file in your vault. Add `[[wikilinks]]` to connect notes
into a knowledge graph.

### Check your stats

```bash
muninn stats
```

Shows snippet, note, and tag counts along with database size.
