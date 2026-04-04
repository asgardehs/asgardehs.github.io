---
layout: default
title: Search
permalink: /docs/muninn/search/
---

[Back to Muninn docs](/docs/muninn/)

# Search

Muninn uses semantic search powered by local embeddings — no API calls, no
network required after the initial model download. You describe what you're
looking for in plain language, and Muninn finds the closest match even if you
used different words when you saved it.

## How it works

When you save a snippet or index a note, Muninn runs the content through a local
embedding model (nomic-embed-text-v1.5) to produce a vector representation. At
search time, your query is embedded the same way and compared against all stored
vectors using cosine similarity.

This means a search for "that btrfs permissions thing" will find a snippet
titled "subvolume set-default requires the ID" — because the meaning is close,
even though the words are different.

Notes are chunked by paragraph so that long documents still return precise
results. The best-matching chunk per note is returned, along with its heading
for context.

If you built with the `fts5` tag, exact string matching is also available
alongside semantic search. Without it, Muninn still works — semantic search is
the primary method.

---

## search

Unified search across both snippets and notes, ranked together by relevance.

```
muninn search <query> [flags]
```

| Flag      | Type   | Default | Description                                 |
| --------- | ------ | ------- | ------------------------------------------- |
| `--scope` | string | `all`   | What to search: `all`, `snippets`, `notes`  |
| `--lang`  | string |         | Filter by programming language              |
| `--tags`  | string |         | Filter by tags (comma-separated, AND logic) |
| `--limit` | int    | 10      | Maximum results                             |

The query is required and can be multiple words.

### Examples

Search everything:

```bash
muninn search "error handling patterns in Go"
```

Search only snippets:

```bash
muninn search --scope snippets --lang go "context cancellation"
```

Search only notes:

```bash
muninn search --scope notes "goldmark frontmatter parsing"
```

Filter by tags:

```bash
muninn search --tags hyprland,ipc "workspace event handler"
```

### Output

Snippets and notes appear together, sorted by relevance score:

```
[snippet 7] workspace IPC handler (score: 0.892)
    lang: go  tags: hyprland, ipc
    func handleWorkspaceEvent(ev Event) {
        switch ev.Type {
        case "workspace":
    ...
---
[note notes/hyprland-ipc.md] Hyprland IPC Protocol (score: 0.841)
    heading: Event Types
    Workspace events fire when the active workspace
    changes or a window moves between workspaces...
---
```

Snippet results show the first 3 lines of content as a preview, with `...` if
there is more. Note results show the heading of the matching chunk and its
preview text.

---

## note search

If you only want to search notes with frontmatter filters, use
`muninn note search` instead — see [Notes](/docs/muninn/notes/#note-search) for details.
That command supports filtering by type, status, area, project, and language.

The unified `search` command does not support frontmatter filters — use
`--scope notes` to limit scope, but use `note search` when you need field-level
filtering.

---

## Tips

- **Be descriptive, not exact.** Semantic search rewards natural language. "that
  thing where nginx drops the host header" works better than "nginx host".
- **Use scope to reduce noise.** If you know it's a snippet, `--scope snippets`
  skips notes and vice versa.
- **Tags are AND logic.** `--tags "go,concurrency"` returns only results tagged
  with both.
- **Re-index after editing notes.** Changes to note files on disk are not
  automatically reflected in search. Run `muninn note index` or let the LSP
  server handle it if you're using the VS Code extension.
