---
layout: docs
title: Notes
permalink: /docs/muninn/notes/
---

[Back to Muninn docs](/docs/muninn/)

# Notes

Notes are markdown files stored in your vault's `notes/` directory. They live on
disk as plain files — edit them in any editor, version them with git, and link
them together with `[[wikilinks]]`.

## Frontmatter

Notes support YAML frontmatter between `---` fences at the top of the file.
These fields can be used as filters in `note list` and `note search`.

| Field      | Values                                                                     | Description                  |
| ---------- | -------------------------------------------------------------------------- | ---------------------------- |
| `type`     | `design-doc`, `til`, `reference`, `decision`, `troubleshooting`, `journal` | What kind of note this is    |
| `status`   | `draft`, `active`, `complete`, `archived`                                  | Lifecycle stage              |
| `area`     | `personal`, `work`, `journal`                                              | Knowledge area               |
| `project`  | any string                                                                 | Associated project name      |
| `language` | any string                                                                 | Primary programming language |
| `tags`     | list of strings                                                            | Categorization tags          |

Example:

```yaml
---
title: Btrfs Subvolume Gotchas
type: til
status: active
area: personal
tags:
  - btrfs
  - linux
---
```

All fields are optional. Notes without frontmatter are still searchable by
their content.

---

## note new

Create a new note with optional frontmatter fields pre-filled.

```
muninn note new <title> [flags]
```

| Flag        | Type   | Default | Description                             |
| ----------- | ------ | ------- | --------------------------------------- |
| `--type`    | string |         | Note type (see frontmatter table above) |
| `--status`  | string |         | Lifecycle status                        |
| `--area`    | string |         | Knowledge area                          |
| `--project` | string |         | Associated project                      |
| `--lang`    | string |         | Primary programming language            |
| `--tags`    | string |         | Comma-separated tags                    |

The title can be multiple words — no quoting required.

### Examples

Simple note:

```bash
muninn note new Btrfs Subvolume Gotchas
```

Note with metadata:

```bash
muninn note new Go Context Patterns --type reference --area work --lang go --tags "concurrency,context"
```

### Output

```
Created btrfs-subvolume-gotchas.md
```

The filename is derived from the title: lowercased, spaces replaced with
hyphens, special characters stripped.

---

## note list

List notes in the vault with optional frontmatter filters.

```
muninn note list [flags]
```

| Flag        | Type   | Default | Description         |
| ----------- | ------ | ------- | ------------------- |
| `--type`    | string |         | Filter by note type |
| `--status`  | string |         | Filter by status    |
| `--area`    | string |         | Filter by area      |
| `--project` | string |         | Filter by project   |
| `--lang`    | string |         | Filter by language  |

### Examples

List all notes:

```bash
muninn note list
```

Filter by type and area:

```bash
muninn note list --type til --area work
```

### Output

```
  btrfs-subvolume-gotchas.md                   Btrfs Subvolume Gotchas
  go-context-patterns.md                       Go Context Patterns
```

---

## note search

Search notes by text with optional frontmatter filters. This is equivalent to
`muninn search` but scoped to the `note` subcommand.

```
muninn note search <query> [flags]
```

| Flag        | Type   | Default | Description         |
| ----------- | ------ | ------- | ------------------- |
| `--limit`   | int    | 10      | Maximum results     |
| `--type`    | string |         | Filter by note type |
| `--status`  | string |         | Filter by status    |
| `--area`    | string |         | Filter by area      |
| `--project` | string |         | Filter by project   |
| `--lang`    | string |         | Filter by language  |

### Examples

```bash
muninn note search "btrfs subvolume permissions"
muninn note search "error handling" --type reference --lang go
```

### Output

```
[btrfs-subvolume-gotchas.md] Btrfs Subvolume Gotchas (score: 7)
    set-default requires the subvol ID, not the path.
```

Results are ranked by relevance: title matches score highest, then tags, then
body content.

---

## note backlinks

Show every note that contains a `[[wikilink]]` pointing to the given note.

```
muninn note backlinks <rel-path>
```

No flags. The relative path is required.

### Example

```bash
muninn note backlinks btrfs-subvolume-gotchas.md
```

### Output

```
Notes linking to "btrfs-subvolume-gotchas":
  linux-filesystem-notes.md
  journal/2026-04-W1-05.md
```

---

## Wikilinks

Notes support `[[wikilinks]]` for connecting your knowledge graph.

| Syntax                                  | Meaning                                           |
| --------------------------------------- | ------------------------------------------------- |
| `[[target]]`                            | Link to `target.md`                               |
| `[[target\|display text]]`              | Link to `target.md`, show "display text"          |
| `[[target#heading]]`                    | Link to a specific heading in `target.md`         |
| `[[target#heading\|display text]]`      | Link to heading, show "display text"              |

Links are case-insensitive and match against filenames (without the `.md`
extension). Heading fragments are also case-insensitive and match against ATX
headings (`# Heading`, `## Section`, etc.) in the target note.

The LSP server provides:
- **Completions** — note names after `[[`, headings after `[[note#`, tags
  after `#`
- **Go to Definition** — jump to the target note or specific heading
- **Hover** — preview the target note content (scoped to heading if fragment
  used)
- **Diagnostics** — warnings for broken links and unresolved heading fragments
- **Rename** — rename a note or heading, updating all references across the
  vault
- **Code Lens** — reference count above note titles
- **Code Actions** — Quick Fix to create a note from a broken link

---

## Callouts

Notes support Obsidian-compatible callout/admonition syntax:

```markdown
> [!warning] Be careful
> This is a warning callout with a title.

> [!tip]
> A tip callout without a custom title.
```

Supported types: `note`, `warning`, `tip`, `info`, `danger`, `success`,
`example`, `quote`, `bug`, `abstract`.

The VS Code extension highlights callout syntax in the editor.

---

## Daily Notes

The VS Code extension includes a daily note command for journaling:

- **Command:** "Muninn: Daily Note" (`Ctrl+Alt+D` / `Cmd+Alt+D`)
- **Quick pick:** Today, Tomorrow, Yesterday, or a custom date
- **File path:** `journal/YYYY-MM-W#-DD.md` (week-of-month numbering)
- **Frontmatter:** Automatically set to `type: journal`, `area: journal`

Custom dates accept `YYYY-MM-DD`, `+N` (days ahead), or `-N` (days back).
