---
layout: default
title: Snippets
permalink: /docs/muninn/snippets/
---

[Back to Muninn docs](/docs/muninn/)

# Snippets

Snippets are the core unit of Muninn's knowledge base — short pieces of code,
commands, notes, or any text you want to save and find later. Unlike notes,
snippets are stored entirely in the database (not as files on disk).

## save

Save a snippet from a file, stdin, or a piped command.

```
muninn save [file | -] [flags]
```

| Flag       | Type   | Default | Description                                                        |
| ---------- | ------ | ------- | ------------------------------------------------------------------ |
| `--title`  | string |         | Short description of the snippet                                   |
| `--lang`   | string |         | Programming language                                               |
| `--tags`   | string |         | Comma-separated tags                                               |
| `--source` | string | auto    | Where the snippet came from (auto-detected as `stdin` or `manual`) |

The argument is either a file path or `-` for stdin. When reading from stdin,
you can pipe content from any command.

### Examples

Save from a file:

```bash
muninn save --title "workspace IPC handler" --lang go --tags "hyprland,ipc" handler.go
```

Save from stdin:

```bash
echo "btrfs subvol set-default requires the subvol ID, not the path" | muninn save --tags btrfs -
```

Pipe from another command:

```bash
sed -n '10,30p' main.go | muninn save --title "error handler" --lang go -
```

### Output

```
Saved snippet 7: workspace IPC handler
```

If no title is given, the output omits it:

```
Saved snippet 7
```

---

## get

Retrieve a snippet's full content and metadata by its ID.

```
muninn get <id>
```

No flags. The ID is required.

### Example

```bash
muninn get 7
```

### Output

```
ID:       7
Title:    workspace IPC handler
Language: go
Tags:     hyprland, ipc
Source:   manual
Created:  2026-03-15 14:30

func handleWorkspaceEvent(ev Event) {
    ...
}
```

---

## list

List snippets with optional filtering and pagination.

```
muninn list [flags]
```

| Flag       | Type   | Default | Description                                 |
| ---------- | ------ | ------- | ------------------------------------------- |
| `--lang`   | string |         | Filter by programming language              |
| `--tags`   | string |         | Filter by tags (comma-separated, AND logic) |
| `--limit`  | int    | 20      | Maximum results to return                   |
| `--offset` | int    | 0       | Offset for pagination                       |

### Examples

List all snippets:

```bash
muninn list
```

Filter by language and tags:

```bash
muninn list --lang go --tags hyprland
```

Page through results:

```bash
muninn list --limit 10 --offset 10
```

### Output

```
Showing 3 of 12 snippets

  7  workspace IPC handler                      [go]
  5  btrfs subvol set-default requires the s…
  3  nginx reverse proxy config                 [nginx]
```

Snippets without a title show the first 60 characters of content instead.

---

## delete

Remove a snippet by its ID. This is permanent.

```
muninn delete <id>
```

No flags. The ID is required.

### Example

```bash
muninn delete 7
```

### Output

```
Deleted snippet 7
```

---

## tag

Add or remove tags from an existing snippet.

```
muninn tag <id> [flags]
```

| Flag       | Type   | Default | Description                      |
| ---------- | ------ | ------- | -------------------------------- |
| `--add`    | string |         | Tags to add (comma-separated)    |
| `--remove` | string |         | Tags to remove (comma-separated) |

Both flags can be used together in a single command.

### Examples

Add tags:

```bash
muninn tag 7 --add "resolved,documented"
```

Remove a tag:

```bash
muninn tag 7 --remove wip
```

Add and remove in one go:

```bash
muninn tag 7 --add resolved --remove wip
```

### Output

```
Snippet 7 tags: hyprland, ipc, resolved, documented
```

If all tags are removed:

```
Snippet 7: no tags
```

---

## tags

List every tag in the database with a count of how many snippets use it.

```
muninn tags
```

No flags.

### Output

```
  btrfs                          2
  go                             5
  hyprland                       3
  ipc                            1
```

---

## export

Export all snippets to a JSON file for backup or migration.

```
muninn export [flags]
```

| Flag           | Type   | Default      | Description      |
| -------------- | ------ | ------------ | ---------------- |
| `-o, --output` | string | `-` (stdout) | Output file path |

### Examples

Export to a file:

```bash
muninn export -o backup.json
```

Export to stdout (for piping):

```bash
muninn export | jq '.snippets | length'
```

### Output

```
Exported 42 snippets to backup.json
```

---

## import

Import snippets from a JSON export file. Duplicates are automatically skipped
based on content hashing.

```
muninn import <file>
```

No flags. The file path is required.

### Example

```bash
muninn import backup.json
```

### Output

```
Imported 38 of 42 snippets (duplicates skipped)
```
