---
layout: docs
title: Setup
permalink: /docs/muninn/server/
---

[Back to Muninn docs](/docs/muninn/)

# Setup

## init

First-time setup. Creates the vault directory and `notes/` folder.

```
muninn init
```

No flags. Run this once after installing the binary.

### What it does

1. Creates the vault directory (see
   [Configuration](/docs/muninn/configuration/) for the default location)
2. Creates the `notes/` subdirectory for your knowledge base

### Output

```
Vault ready at /home/you/.local/share/muninn
  notes/ directory: /home/you/.local/share/muninn/notes

Run `cd /home/you/.local/share/muninn && git init` to version your notes.
```

After init, you can start creating notes immediately.

---

## install

Install the Muninn VS Code extension.

```
muninn install
```

No flags. Looks for `muninn-0.1.0.vsix` near the binary. If you built from
source, make sure you've
[built the extension](/docs/muninn/requirements/#vs-code-extension) first.

The extension launches the LSP server in the background and provides wikilink
completions, go-to-definition, backlinks, hover previews, broken link
diagnostics, and the "New Note" command (`Ctrl+Shift+N`).

### Output

```
  VS Code: installed muninn-0.1.0.vsix
  Restart VS Code to activate.
```

---

## lsp

Start the LSP (Language Server Protocol) server. This is what the VS Code
extension launches behind the scenes.

```
muninn lsp
```

No flags. Communicates over stdio using the LSP protocol.

You don't need to run this directly — the VS Code extension manages it. It's
useful for editor integrations beyond VS Code (Neovim, Helix, etc.) that
support LSP over stdio.

### Features

- **Completions** — note names after `[[`, headings after `[[note#`, and tags
  after `#`
- **Go to Definition** — jump to target note or specific heading from
  `[[note#heading]]`
- **References** — find all notes that link to the current note (backlinks)
- **Hover** — preview a linked note's content, scoped to the heading if a
  fragment is used
- **Diagnostics** — warnings on broken `[[links]]` and unresolved
  `[[note#heading]]` fragments
- **Document Symbols** — heading outline in the sidebar (breadcrumbs + Outline
  panel)
- **Workspace Symbols** — Ctrl+T to search across all note titles and headings
- **Semantic Tokens** — LSP-driven highlighting for resolved/broken wikilinks
  and tags
- **Code Lens** — reference count shown above each note title
- **Code Actions** — Quick Fix to create a note from a broken wikilink
- **Rename** — rename a note or heading and update all wikilinks across the
  vault
