---
layout: docs
title: Schema Builder
permalink: /docs/odin/schema-builder/
---

[Back to Odin docs](/docs/odin/)

# Schema Builder

The Schema Builder lets users design their own database tables, fields, and
relationships from scratch — runtime DDL on SQLite with safety, auditability,
and interoperability with pre-built modules.

## Metadata Model

Three metadata tables describe user-defined schemas:

- **`_custom_tables`** — table definitions (name, display name, icon,
  establishment scope)
- **`_custom_fields`** — field definitions (name, type, required, default,
  type-specific config JSON)
- **`_custom_relations`** — relationships between tables (belongs_to, has_many,
  many_to_many; can target custom or pre-built tables)
- **`_custom_table_versions`** — DDL change history for each table

## Field Types

| Schema Builder Type | SQLite Type | Config Examples                                    |
| ------------------- | ----------- | -------------------------------------------------- |
| `text`              | TEXT        | `{"multiline": true, "max_length": 500}`           |
| `number`            | INTEGER     | `{"min": 0, "max": 100}`                          |
| `decimal`           | REAL        | `{"precision": 2}`                                 |
| `date`              | TEXT        | stored as YYYY-MM-DD                               |
| `datetime`          | TEXT        | stored as ISO 8601                                 |
| `boolean`           | INTEGER     | 0/1                                                |
| `select`            | TEXT        | `{"options": ["Low", "Medium", "High"]}`           |
| `relation`          | INTEGER     | `{"target_table": "cx_projects", "display_field": "name"}` |

## Naming and Safety

All user-created tables get the `cx_` prefix. The validator enforces:

- Table names: `^[a-z][a-z0-9_]{1,58}$` (after prefix, max 63 chars)
- Field names: `^[a-z][a-z0-9_]{1,62}$`
- Reserved names blocked: `id`, `establishment_id`, `created_at`, `updated_at`
  (auto-added to every custom table)
- Cannot collide with pre-built table names
- Relation targets must exist

## DDL Execution

The executor translates metadata into DDL:

**Table creation** adds `id`, `establishment_id`, `created_at`, `updated_at`
automatically, plus an index on `establishment_id`.

**Field addition** uses `ALTER TABLE ADD COLUMN`.

**Field deactivation** sets `is_active = 0` in metadata — hides from UI and
query builder. SQLite column remains. Explicit compaction can drop it later
(SQLite 3.35.0+).

## Dynamic Query Builder

Safe parameterized queries for custom tables. Column names validated against
metadata are interpolated; all data values are parameterized:

```go
func (qb *QueryBuilder) Select(tableID int64, opts SelectOpts) (string, []any, error)
func (qb *QueryBuilder) Insert(tableID int64, values map[string]any) (string, []any, error)
func (qb *QueryBuilder) Update(tableID int64, id int64, values map[string]any) (string, []any, error)
func (qb *QueryBuilder) Delete(tableID int64, id int64) (string, []any, error)
```

## Interoperability

Custom tables can relate to whitelisted pre-built tables:

- `establishments`, `employees`
- `incidents`, `chemicals`
- `training_courses`, `training_completions`
- `storage_locations`, `work_areas`

The query builder generates appropriate JOINs. `RecordForm.tsx` renders
searchable dropdowns for relation fields.

## Table Designer UI

1. **Table metadata** — display name, description, icon
2. **Fields list** — drag-to-reorder, type, required, config
3. **Relations panel** — source field, target table, relation type
4. **Preview** — live record form preview from current field definitions
5. **Save** — diffs against current state, executes necessary DDL

## Schema Versioning

Every DDL change is recorded in `_custom_table_versions` — enables future undo,
export, and schema sharing between establishments.
