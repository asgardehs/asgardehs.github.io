---
layout: default
title: Command Reference
permalink: /docs/muninn/commands/
---

[Back to Muninn docs](/docs/muninn/)

# Command Reference

Muninn's CLI is organized into command groups. Each page below covers one group
in detail with flags, examples, and expected output.

| Command          | Description                                        | Page                                              |
| ---------------- | -------------------------------------------------- | ------------------------------------------------- |
| `save`           | Save a snippet from a file or stdin                | [Snippets](/docs/muninn/snippets/)                |
| `get`            | Retrieve a snippet by ID                           | [Snippets](/docs/muninn/snippets/)                |
| `list`           | List snippets with filters                         | [Snippets](/docs/muninn/snippets/)                |
| `delete`         | Remove a snippet                                   | [Snippets](/docs/muninn/snippets/)                |
| `tag`            | Add or remove tags on a snippet                    | [Snippets](/docs/muninn/snippets/)                |
| `tags`           | List all tags with counts                          | [Snippets](/docs/muninn/snippets/)                |
| `export`         | Export all snippets to JSON                        | [Snippets](/docs/muninn/snippets/)                |
| `import`         | Import snippets from JSON                          | [Snippets](/docs/muninn/snippets/)                |
| `search`         | Unified semantic search                            | [Search](/docs/muninn/search/)                    |
| `note new`       | Create a new note                                  | [Notes](/docs/muninn/notes/)                      |
| `note list`      | List notes with filters                            | [Notes](/docs/muninn/notes/)                      |
| `note index`     | Index notes for search                             | [Notes](/docs/muninn/notes/)                      |
| `note search`    | Semantic search across notes                       | [Notes](/docs/muninn/notes/)                      |
| `note backlinks` | Show notes linking to a given note                 | [Notes](/docs/muninn/notes/)                      |
| `init`           | Create vault, database, and download model         | [Server & Setup](/docs/muninn/server/)            |
| `install`        | Register with Claude Desktop, Claude Code, VS Code | [Server & Setup](/docs/muninn/server/)            |
| `serve`          | Start MCP server                                   | [Server & Setup](/docs/muninn/server/)            |
| `lsp`            | Start LSP server                                   | [Server & Setup](/docs/muninn/server/)            |
| `stats`          | Show knowledge base statistics                     | [Server & Setup](/docs/muninn/server/)            |
| `import-notes`   | Import markdown notes from external directories    | [Notes](/docs/muninn/notes/)                      |

See also: [Configuration](/docs/muninn/configuration/) for environment variables and data
locations.
