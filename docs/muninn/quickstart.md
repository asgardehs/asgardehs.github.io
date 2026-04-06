---
layout: default
title: Quick Start
permalink: /docs/muninn/quickstart/
---

[Back to Muninn docs](/docs/muninn/)

# Quick Start

After [building](/docs/muninn/requirements/) and adding the binary to your
`PATH`, run:

```bash
muninn init
```

This creates the vault directory and `notes/` folder. Muninn is ready to use.

## Install the VS Code Extension

```bash
muninn install
```

This installs the `.vsix` extension that ships with Muninn. It launches the LSP
server in the background and gives you:

- `[[wikilink]]` completions and syntax highlighting in markdown
- Go to Definition on wikilinks (jump to the linked note)
- Backlink references (find every note that links to the current one)
- Hover previews of linked notes
- Diagnostics for broken links
- A "New Note" command (`Ctrl+Shift+N`)

The extension requires the Muninn binary on your `PATH` (or configured via the
`muninn.binaryPath` setting).

> **Note:** The `install` command looks for a pre-built `muninn-0.1.0.vsix`
> near the binary. If you built from source, make sure you've
> [built the extension](/docs/muninn/requirements/#vs-code-extension) first.

## First Steps

### Create a note

```bash
muninn note new "Btrfs Subvolume Gotchas" --tags "btrfs,linux"
```

Opens a new markdown file in your vault with frontmatter pre-filled.
Add `[[wikilinks]]` to connect notes into a knowledge graph.

### Find it later

```bash
muninn search "btrfs subvolume permissions"
```

Text search matches against note titles, tags, and content.

### List your notes

```bash
muninn note list
muninn note list --type til --area work
```

### Check backlinks

```bash
muninn note backlinks btrfs-subvolume-gotchas.md
```

Shows every note that contains a `[[wikilink]]` to the given note.
