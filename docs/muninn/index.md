---
layout: docs
title: Muninn Documentation
description: Hierarchical notes for VS Code — fuzzy lookup, schema-driven creation, wikilinks.
permalink: /docs/muninn/
---

Muninn is a personal knowledge management (PKM) extension for VS Code and VSCodium. Files named with dot-paths (`projects.alpha.kickoff.md`) form a hierarchy; missing intermediates appear in the tree as clickable stubs. Wikilinks, backlinks, and schema-driven note creation are powered by an LSP server in a Go sidecar.

> **Companion project.** Muninn is not part of the Asgard EHS suite. It runs in your editor, stores plain markdown on disk, and works without the rest of the Asgard tools. The bundled `ehs` schema pack is useful if you happen to author EHS notes — but that's the only overlap.

## Why Muninn

- **Plain markdown on disk.** No database, no embeddings, no telemetry. Every note is a `.md` file you can open with anything.
- **Hierarchy from filenames.** Dot-paths make the tree explicit — moving a subtree is `git mv`, not a UI dance.
- **Schema engine.** YAML schemas in `<vault>/.muninn/schemas/` constrain frontmatter, enumerate vocabularies, and template new notes. Violations surface in the Problems panel.
- **Wikilinks via LSP.** `[[target]]` completion, hover preview, go-to-definition, find-all-references, broken-link diagnostics, code-lens reference counts.

## When not to use Muninn

- **You need block references or transclusion.** Muninn links at the note level. If you live in Roam or Logseq's block model, Muninn will feel coarse.
- **You don't use VS Code or a VSCodium fork.** Muninn ships as a VS Code extension. There is no standalone CLI or web app.
- **You want a hosted second brain.** Muninn is local-first by design. Your vault syncs by however you sync files (git, Syncthing, Dropbox).

## Architecture

Muninn is two pieces wired through one stdio pipe:

- **Go sidecar** (`muninn-sidecar`) — vault I/O, wikilink index, hierarchy tree, schema engine, LSP server. Static binary, no native dependencies.
- **TypeScript extension host** — spawns the sidecar, hosts the lookup palette and tree view, bridges VS Code's `vscode-languageclient` to the sidecar's LSP channel. Deliberately thin.

LSP and a custom JSON-RPC channel share the pipe via a `Channel:` header on top of LSP framing. No databases, no embeddings, no network calls.

## Getting started

- [Quick start](/docs/muninn/quickstart/) — install from Open VSX, sideload a VSIX, open a vault, run your first lookup.

## Source

The canonical README, full feature tour, and command reference live on GitHub:
[github.com/asgardehs/muninn-vscode](https://github.com/asgardehs/muninn-vscode)
