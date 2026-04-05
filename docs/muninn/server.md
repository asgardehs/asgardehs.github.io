---
layout: default
title: Server & Setup
permalink: /docs/muninn/server/
---

[Back to Muninn docs](/docs/muninn/)

# Server & Setup

Muninn runs as three different servers depending on the context: a CLI for
direct use, an MCP server for AI assistants, and an LSP server for editor
integration. This page covers the commands that set up and run these servers.

## init

First-time setup. Creates the vault directory, database, `notes/` folder, and
downloads the embedding model.

```
muninn init
```

No flags. Run this once after installing the binary.

### What it does

1. Creates the vault directory (see [Configuration](/docs/muninn/configuration/) for the
   default location)
2. Creates the `notes/` subdirectory for your knowledge base
3. Creates and migrates the SQLite database (`muninn.db`)
4. Generates a `.gitignore` that excludes the database and model cache
5. Downloads the embedding model (~130 MB, one-time)

### Output

```
Creating vault at /home/you/.local/share/muninn ...
  Created notes/ directory
  Created muninn.db
  Downloading embedding model (nomic-ai/nomic-embed-text-v1.5)...
    model.onnx: 100%
  Model cached

Ready. Run `cd /home/you/.local/share/muninn && git init` to version your notes.
```

After init, you can start saving snippets and creating notes immediately.

---

## install

Register Muninn with Claude Desktop, Claude Code, and/or VS Code so they can use
it automatically.

```
muninn install [flags]
```

| Flag        | Type | Default | Description                  |
| ----------- | ---- | ------- | ---------------------------- |
| `--desktop` | bool | false   | Register with Claude Desktop |
| `--code`    | bool | false   | Register with Claude Code    |
| `--vscode`  | bool | false   | Install VS Code extension    |
| `--force`   | bool | false   | Overwrite existing entries   |

With no flags, all three are attempted. Use flags to target specific tools.

### Claude Desktop (`--desktop`)

```bash
muninn install --desktop
```

Writes a server entry to Claude Desktop's JSON config file. The config location
is platform-dependent:

| Platform | Config path                                                       |
| -------- | ----------------------------------------------------------------- |
| Linux    | `~/.config/claude-desktop/claude_desktop_config.json`             |
| macOS    | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows  | `%APPDATA%\Claude\claude_desktop_config.json`                     |

After installing, restart Claude Desktop. Muninn's 9 tools will appear in
conversations — Claude can save snippets, search your knowledge base, create and
read notes, and manage tags on your behalf.

### Claude Code (`--code`)

```bash
muninn install --code
```

Same as above, but for Claude Code (the CLI/editor agent). Writes the entry to
`~/.claude.json`. This lets Claude Code access your knowledge base during
terminal sessions — useful for recalling snippets, referencing past decisions,
or saving context without leaving the terminal.

### VS Code (`--vscode`)

```bash
muninn install --vscode
```

Installs the `.vsix` extension that ships with Muninn. The command looks for
`muninn-0.1.0.vsix` near the binary. If you built from source, make sure you've
[built the extension](/docs/muninn/requirements/#vs-code-extension) first.

The extension launches the LSP server in the background and provides wikilink
completions, go-to-definition, backlinks, hover previews, broken link
diagnostics, and the "New Note" command (`Ctrl+Shift+N`).

### Overwriting existing entries

If Muninn is already registered, the command will skip it. Use `--force` to
overwrite:

```bash
muninn install --force
```

This is useful after rebuilding the binary at a new path.

### Output

```
  Claude Desktop: wrote muninn server to /home/you/.config/claude-desktop/claude_desktop_config.json
  Claude Code: wrote muninn server to /home/you/.claude.json
  VS Code: installed muninn-0.1.0.vsix
  Restart clients to activate.
```

---

## serve

Start the MCP (Model Context Protocol) server. This is what Claude Desktop and
Claude Code launch behind the scenes after `muninn install`.

```
muninn serve
```

No flags. Communicates over stdio using the MCP protocol.

The server automatically exits after 15 minutes of inactivity (no stdin data) to
free resources. MCP clients restart it transparently on the next request.

You generally don't need to run this yourself — `muninn install` configures your
AI tools to launch it automatically. It's useful for debugging or if you want to
connect a custom MCP client.

### Exposed tools

| Tool                  | Description                                            |
| --------------------- | ------------------------------------------------------ |
| `muninn_save`         | Save a snippet with optional title, language, and tags |
| `muninn_search`       | Unified semantic search across snippets and notes      |
| `muninn_get`          | Retrieve a specific snippet by ID                      |
| `muninn_list`         | List snippets with optional filters                    |
| `muninn_delete`       | Remove a snippet                                       |
| `muninn_tag`          | Add or remove tags from a snippet                      |
| `muninn_note_create`  | Create a new note with frontmatter                     |
| `muninn_note_search`  | Search notes with frontmatter filters                  |
| `muninn_note_get`     | Read a note's full content and backlinks               |
| `muninn_note_index`   | Trigger vault re-indexing                              |
| `muninn_note_links`   | Get forward links and backlinks with fragment details  |
| `muninn_note_graph`   | Traverse the wikilink graph from a starting note       |

See the [MCP Tools Reference](/docs/muninn/mcp-tools/) for full parameter
documentation.

---

## lsp

Start the LSP (Language Server Protocol) server. This is what the VS Code
extension launches behind the scenes.

```
muninn lsp
```

No flags. Communicates over stdio using the LSP protocol.

Like `serve`, you don't need to run this directly — the VS Code extension
manages it. It's useful for editor integrations beyond VS Code (Neovim, Helix,
etc.) that support LSP over stdio.

### Features

- **Completions** — note names after `[[`, headings after `[[note#`, and tags after `#`
- **Go to Definition** — jump to target note or specific heading from `[[note#heading]]`
- **References** — find all notes that link to the current note (backlinks)
- **Hover** — preview a linked note's content, scoped to the heading if a fragment is used
- **Diagnostics** — warnings on broken `[[links]]` and unresolved `[[note#heading]]` fragments
- **Document Symbols** — heading outline in the sidebar (breadcrumbs + Outline panel)
- **Workspace Symbols** — Ctrl+T to search across all note titles and headings
- **Semantic Tokens** — LSP-driven highlighting for resolved/broken wikilinks and tags
- **Code Lens** — reference count shown above each note title
- **Code Actions** — Quick Fix to create a note from a broken wikilink
- **Rename** — rename a note or heading and update all wikilinks across the vault
- **File Watching** — re-indexes changed files and updates diagnostics

---

## stats

Show a summary of what's in your knowledge base.

```
muninn stats
```

No flags.

### Output

```
Snippets:   42
Notes:      15
Chunks:     87
Tags:       23
Wikilinks:  31
DB size:    4.20 MB
```

- **Chunks** — the number of paragraph-level embeddings stored for notes (each
  note produces multiple chunks for precise search)
- **Wikilinks** — the total number of `[[link]]` references across all notes
- **DB size** — the size of `muninn.db` on disk
