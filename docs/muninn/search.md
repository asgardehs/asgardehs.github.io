---
layout: default
title: Search
permalink: /docs/muninn/search/
---

[Back to Muninn docs](/docs/muninn/)

# Search

Muninn uses text search to find notes in your vault. Your query is split into
words and matched against note titles, tags, and content. No external services
or models required — search works entirely on the local filesystem.

## How it works

When you search, Muninn walks every note in the vault and scores it against
your query:

- **Title match** — highest weight. A word appearing in the title scores 3
  points.
- **Tag match** — medium weight. A word matching a frontmatter tag scores 2
  points.
- **Body match** — a word appearing anywhere in the note content scores 1
  point.

Results are ranked by total score and the top matches are returned with a
preview line showing the first relevant match in the body.

---

## search

Search across all notes in the vault.

```
muninn search <query> [flags]
```

| Flag        | Type   | Default | Description         |
| ----------- | ------ | ------- | ------------------- |
| `--limit`   | int    | 10      | Maximum results     |
| `--type`    | string |         | Filter by note type |
| `--status`  | string |         | Filter by status    |
| `--area`    | string |         | Filter by area      |
| `--project` | string |         | Filter by project   |
| `--lang`    | string |         | Filter by language  |

The query is required and can be multiple words.

### Examples

Basic search:

```bash
muninn search "btrfs subvolume permissions"
```

Filtered search:

```bash
muninn search "error handling" --type reference --lang go
```

### Output

```
[btrfs-subvolume-gotchas.md] Btrfs Subvolume Gotchas (score: 7)
    set-default requires the subvol ID, not the path.
---
[go-error-patterns.md] Go Error Patterns (score: 4)
    Always wrap errors with fmt.Errorf to preserve context...
```

---

## note search

The `note search` subcommand is equivalent to `search` — both search the same
vault with the same flags. Use whichever feels natural:

```bash
muninn search "btrfs permissions"
muninn note search "btrfs permissions"
```

See [Notes — note search](/docs/muninn/notes/#note-search) for details.

---

## Tips

- **Use multiple words.** More words give more signal for scoring. "btrfs
  subvolume permissions" is better than just "btrfs".
- **Frontmatter filters narrow results.** Use `--type`, `--area`, `--lang`,
  etc. to focus on a specific slice of your vault.
- **Scores are simple.** A note that matches your query in the title, tags,
  _and_ body will score higher than one that only matches in the body.
