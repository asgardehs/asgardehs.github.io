---
layout: default
title: Notes
permalink: /docs/muninn/notes/
---

[Back to Muninn docs](/docs/muninn/)

# Notes

Notes are markdown files stored in your vault's `notes/` directory. Unlike
snippets, notes live on disk as real files — you can edit them in any editor,
version them with git, and link them together with `[[wikilinks]]`.

Muninn indexes notes into the same database used by snippets, so both are
searchable from a single query.

## Frontmatter

Notes support YAML frontmatter between `---` fences at the top of the file.
These fields are indexed and can be used as filters in `note list` and
`note search`.

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

All fields are optional. Notes without frontmatter are still indexed by their
content.

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

Journal entry:

```bash
muninn note new "2026-04-02 standup" --type journal --area work --project muninn
```

### Output

```
Created notes/btrfs-subvolume-gotchas.md
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
  notes/btrfs-subvolume-gotchas.md             Btrfs Subvolume Gotchas
  notes/go-context-patterns.md                 Go Context Patterns
  notes/2026-04-02-standup.md                  2026-04-02 standup
```

---

## note index

Index notes so they become searchable. Muninn embeds note content into vectors
for semantic search. Unchanged notes are skipped automatically (content
hashing).

```
muninn note index [rel-path]
```

No flags. The relative path is optional — omit it to index the entire vault.

### Examples

Index everything:

```bash
muninn note index
```

Index a single note:

```bash
muninn note index btrfs-subvolume-gotchas.md
```

### Output

Bulk index:

```
Indexed 15 of 15 notes
```

Single file:

```
Indexed btrfs-subvolume-gotchas.md
```

Notes with errors (bad frontmatter, unreadable files) are skipped and reported
to stderr.

---

## note search

Semantic search across your notes. This searches note content and returns the
most relevant chunk from each matching note.

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

Basic search:

```bash
muninn note search "btrfs subvolume permissions"
```

Filtered search:

```bash
muninn note search "error handling" --type reference --lang go
```

### Output

```
[notes/btrfs-subvolume-gotchas.md] Btrfs Subvolume Gotchas (score: 0.847)
    heading: Common Pitfalls
    set-default requires the subvol ID, not the path.
    Use `btrfs subvol list /` to find the correct ID...
---
```

The `heading` line shows which section of the note matched. The preview shows
the most relevant chunk.

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
  notes/linux-filesystem-notes.md
  notes/2026-03-20-debugging-session.md
```

---

## Wikilinks

Notes support `[[wikilinks]]` for connecting your knowledge graph.

| Syntax                       | Meaning                                  |
| ---------------------------- | ---------------------------------------- |
| `[[target]]`                 | Link to `target.md`                      |
| `[[target\|display text]]`   | Link to `target.md`, show "display text" |

Links are case-insensitive and match against filenames (without the `.md`
extension). The LSP server provides completions, go-to-definition, hover
previews, and diagnostics for broken links.
