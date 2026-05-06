---
layout: docs
title: Muninn Quick Start
description: Install Muninn, open a vault, and run your first lookup.
permalink: /docs/muninn/quickstart/
---

## Install

### Open VSX (VSCodium, Cursor, Theia, Gitpod, code-server)

Muninn is published to the [Open VSX Registry](https://open-vsx.org/extension/pharomwinters/muninn) as `pharomwinters.muninn`. Install through your editor's extensions marketplace.

### Stock VS Code

Stock VS Code uses Microsoft's marketplace, which Muninn isn't published to. Sideload the VSIX from [GitHub Releases](https://github.com/asgardehs/muninn-vscode/releases):

1. Download the latest `muninn-*.vsix`.
2. In VS Code, open the **Extensions** view, click the **…** menu, and choose **Install from VSIX…**.
3. Select the file you downloaded.

### Building from source

See the [contributor guide](https://github.com/asgardehs/muninn-vscode/blob/main/CONTRIBUTING.md) on GitHub.

## Open a vault

A Muninn vault is any folder containing markdown files. There's no init command — open the folder in VS Code and the extension activates.

For schema support, create `<vault>/.muninn/schemas/`. The bundled `generic` schema pack auto-loads as a fallback if no user schemas are present.

## Your first lookup

Press <kbd>Ctrl</kbd>+<kbd>L</kbd> (or <kbd>⌘</kbd>+<kbd>L</kbd> on macOS) to open the lookup palette. Type a dot-path:

```
projects.alpha.kickoff
```

If the note exists, Muninn opens it. If not, Muninn creates `projects.alpha.kickoff.md` and any missing parent stubs in the tree.

To prefill the palette with the active note's parent path, run **Muninn: Lookup Under Current Note** from the command palette.

## Schema packs

Run **Muninn: Install Schema Pack** from the command palette to copy a bundled pack into your vault.

- `generic` — daily notes, meetings, reference, decisions, til
- `ehs` — incident, JHA, inspection, training, audit

User schemas in `.muninn/schemas/` layer on top of the bundled `generic` pack rather than replacing it, so dropping a custom schema doesn't silently delete the defaults you were relying on.

## Next steps

- The [GitHub README](https://github.com/asgardehs/muninn-vscode) has the full command list, settings reference, and architecture notes.
- File issues at [github.com/asgardehs/muninn-vscode/issues](https://github.com/asgardehs/muninn-vscode/issues).
- Release notes live in the [CHANGELOG](https://github.com/asgardehs/muninn-vscode/blob/main/CHANGELOG.md).
