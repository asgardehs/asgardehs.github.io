---
layout: default
title: MCP Tools Reference
permalink: /docs/muninn/mcp-tools/
---

[Back to Muninn docs](/docs/muninn/)

# MCP Tools Reference

When Muninn runs as an MCP daemon (`muninn daemon`), it exposes tools that AI
assistants can use to interact with your knowledge base. This page documents
every available tool with its parameters and behavior.

---

## Snippet Tools

### muninn_save

Save a snippet to the knowledge base.

| Parameter  | Type     | Required | Description                          |
| ---------- | -------- | -------- | ------------------------------------ |
| `content`  | string   | yes      | The snippet text                     |
| `title`    | string   | no       | Short description                    |
| `language` | string   | no       | Programming language                 |
| `tags`     | string[] | no       | Tag names for categorization         |
| `source`   | string   | no       | Where it came from (defaults to mcp) |

### muninn_search

Unified semantic search across snippets and notes. Uses vector embeddings for
meaning-based matching, blended with full-text search for short keyword queries.

| Parameter  | Type     | Required | Description                                    |
| ---------- | -------- | -------- | ---------------------------------------------- |
| `query`    | string   | yes      | Natural language search query                  |
| `scope`    | string   | no       | `all` (default), `snippets`, or `notes`        |
| `language` | string   | no       | Filter snippets by language                    |
| `tags`     | string[] | no       | Filter snippets by tags (AND logic)            |
| `limit`    | number   | no       | Max results (default 10)                       |

### muninn_get

Retrieve a specific snippet by ID.

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `id`      | number | yes      | Snippet ID  |

### muninn_list

List snippets with optional filtering.

| Parameter  | Type     | Required | Description                       |
| ---------- | -------- | -------- | --------------------------------- |
| `language` | string   | no       | Filter by language                |
| `tags`     | string[] | no       | Filter by tags                    |
| `limit`    | number   | no       | Max results (default 20)          |
| `offset`   | number   | no       | Offset for pagination             |

### muninn_delete

Remove a snippet.

| Parameter | Type   | Required | Description          |
| --------- | ------ | -------- | -------------------- |
| `id`      | number | yes      | Snippet ID to delete |

### muninn_tag

Add or remove tags from a snippet.

| Parameter | Type     | Required | Description    |
| --------- | -------- | -------- | -------------- |
| `id`      | number   | yes      | Snippet ID     |
| `add`     | string[] | no       | Tags to add    |
| `remove`  | string[] | no       | Tags to remove |

---

## Note Tools

### muninn_note_create

Create a new note with frontmatter in the vault. The note is saved as a
markdown file and automatically indexed for search.

| Parameter     | Type     | Required | Description                                                              |
| ------------- | -------- | -------- | ------------------------------------------------------------------------ |
| `title`       | string   | yes      | Note title                                                               |
| `tags`        | string[] | no       | Categorization tags                                                      |
| `type`        | string   | no       | Content type: design-doc, til, reference, decision, troubleshooting, journal |
| `status`      | string   | no       | Lifecycle: draft, active, complete, archived                             |
| `area`        | string   | no       | Knowledge area: personal, work, journal                                  |
| `project`     | string   | no       | Associated project name                                                  |
| `language`    | string   | no       | Primary programming language                                             |
| `description` | string   | no       | Short summary                                                            |

### muninn_note_search

Semantic search across knowledge base notes. Supports filtering by frontmatter
fields to narrow results.

| Parameter  | Type   | Required | Description                               |
| ---------- | ------ | -------- | ----------------------------------------- |
| `query`    | string | yes      | Natural language search query              |
| `type`     | string | no       | Filter by content type                     |
| `status`   | string | no       | Filter by lifecycle status                 |
| `area`     | string | no       | Filter by knowledge area                   |
| `project`  | string | no       | Filter by project                          |
| `language` | string | no       | Filter by programming language             |
| `limit`    | number | no       | Max results (default 10)                   |

### muninn_note_get

Read a note's full content with backlinks listed at the bottom.

| Parameter  | Type   | Required | Description                   |
| ---------- | ------ | -------- | ----------------------------- |
| `rel_path` | string | yes      | Path relative to vault root   |

### muninn_note_index

Trigger re-indexing of the vault. Pass a path to reindex a single file, or
omit to reindex all notes.

| Parameter  | Type   | Required | Description                                   |
| ---------- | ------ | -------- | --------------------------------------------- |
| `rel_path` | string | no       | Index single file; omit for full reindex       |

---

## Wikilink Graph Tools

These tools let AI assistants explore the connections between notes. When a user
creates `[[wikilinks]]` between notes, they are recording relationships they
consider meaningful. These tools let an assistant follow those relationships to
build context.

### muninn_note_links

Get all forward links and backlinks for a single note, including heading
fragment targets and display aliases.

| Parameter  | Type   | Required | Description                 |
| ---------- | ------ | -------- | --------------------------- |
| `rel_path` | string | yes      | Path relative to vault root |

**Output format:**

```
Forward links (this note links to):
  → architecture-overview#layers
  → api-design [alias: the API spec]

Backlinks (notes linking here):
  ← sprint-plan.md [via #Authentication]
  ← onboarding-guide.md
```

Use this tool when you need to understand a single note's immediate
neighborhood — what it references and what references it.

### muninn_note_graph

Traverse the wikilink graph starting from a note using breadth-first search.
Discovers all connected notes up to a configurable depth, handling cycles
automatically.

| Parameter   | Type   | Required | Description                                           |
| ----------- | ------ | -------- | ----------------------------------------------------- |
| `rel_path`  | string | yes      | Starting note path                                    |
| `depth`     | number | no       | Max traversal depth (default 2, max 5)                |
| `direction` | string | no       | `forward`, `backward`, or `both` (default `both`)     |

**Output format:**

```
Graph from: my-design-doc.md — "My Design Doc" (depth: 2, direction: both)

[depth 0] my-design-doc.md — "My Design Doc"
  → architecture-overview.md — "Architecture Overview" [via [[Architecture Overview]]]
  → api-design.md — "API Design" [via [[API Design#Auth]]]
  ← sprint-plan.md — "Sprint Plan" [links here via [[My Design Doc]]]

[depth 1] architecture-overview.md — "Architecture Overview"
  → database-schema.md — "Database Schema" [via [[Database Schema]]]

[depth 1] api-design.md — "API Design"
  (already visited)

[depth 1] sprint-plan.md — "Sprint Plan"
  → milestone-3.md — "Milestone 3" [via [[Milestone 3]]]

[depth 2] database-schema.md — "Database Schema"
[depth 2] milestone-3.md — "Milestone 3"

5 notes discovered across 3 levels.
```

**Recommended workflow for AI assistants:**

1. Use `muninn_note_graph` to discover the neighborhood of a topic
2. Identify the most relevant connected notes from the graph output
3. Use `muninn_note_get` to read the full content of those notes
4. Synthesize the information across notes to answer the user's question

This approach lets you follow the same knowledge paths the user built through
their wikilinks, giving you the full picture of how they think about a topic.

---

## Usage Notes

All tools are served by the Muninn daemon (`muninn daemon`) over HTTP. After
running `muninn install`, Claude Desktop and Claude Code connect to the daemon
at `http://localhost:21700/mcp`. Start the daemon before using MCP clients.

Tool parameters that accept numbers (`id`, `limit`, `offset`, `depth`) are
flexible — they accept both JSON numbers and string representations of numbers
to handle different client serialization behaviors.
