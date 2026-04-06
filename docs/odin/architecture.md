---
layout: default
title: Odin Architecture
permalink: /docs/odin/architecture/
---

[Back to Odin docs](/docs/odin/)

# Architecture

## Project Structure

```
odin/
├── main.go                          # HTTP server, service wiring, browser launch
├── go.mod / go.sum
├── internal/
│   ├── server/
│   │   ├── server.go                # HTTP server, router, middleware
│   │   ├── routes.go                # API route registration
│   │   └── handlers/                # HTTP handlers (thin, call services)
│   │       ├── app.go               # Settings, establishment switching
│   │       ├── incidents.go
│   │       ├── chemicals.go
│   │       ├── training.go
│   │       ├── schema.go
│   │       └── reports.go
│   ├── database/
│   │   ├── db.go                    # DB wrapper: Open, WAL, foreign keys
│   │   ├── migrate.go               # Migration runner with version tracking
│   │   └── seed.go                  # Reference data seeding
│   ├── module/
│   │   ├── incidents/               # OSHA 300/300A/301
│   │   │   ├── repository.go
│   │   │   ├── service.go
│   │   │   ├── models.go
│   │   │   └── reports.go
│   │   ├── chemicals/               # HazCom, Tier II, SARA 313/TRI
│   │   ├── training/                # Multi-regulatory training management
│   │   └── registry.go              # Module enable/disable per establishment
│   ├── schema/                      # Schema Builder engine
│   │   ├── meta.go                  # TableDef, FieldDef, RelationDef
│   │   ├── executor.go              # DDL from metadata
│   │   ├── validator.go             # Name rules, type checking
│   │   ├── query.go                 # Dynamic parameterized query builder
│   │   └── service.go               # Orchestrator
│   ├── audit/                       # Change audit log
│   ├── establishment/               # Facility + employee management
│   ├── report/                      # Report engine, registry, CSV export
│   └── platform/                    # XDG paths, DB backup, browser launch
├── frontend/
│   ├── index.html
│   ├── package.json
│   ├── vite.config.ts
│   ├── tailwind.config.js
│   ├── tsconfig.json
│   ├── src/
│   │   ├── main.tsx                 # React mount
│   │   ├── App.tsx                  # Root: router + layout shell
│   │   ├── app.css                  # Tailwind base + custom tokens
│   │   ├── api/
│   │   │   └── client.ts            # Typed HTTP client for Go API
│   │   ├── hooks/
│   │   │   ├── useEstablishment.ts  # Current establishment context
│   │   │   └── useModules.ts        # Enabled modules for current establishment
│   │   ├── components/
│   │   │   ├── layout/              # Shell, Sidebar, Topbar
│   │   │   ├── shared/              # DataTable, FormField, Modal, etc.
│   │   │   └── reports/
│   │   └── pages/                   # Dashboard, incidents/, chemicals/, etc.
│   └── dist/                        # Vite build output (embedded by Go)
└── embed/
    └── migrations/                  # Embedded SQL migration files
        ├── 000_core.sql
        ├── 001_incidents.sql
        ├── 002_chemicals.sql
        ├── 003_training.sql
        └── 100_schema_builder.sql
```

---

## How It Runs

```
┌───────────────────────────────────┐
│        System Browser             │
│   (Chrome / Edge / Firefox)       │
│   http://odin.localhost:8080      │
│                                   │
│   React + Tailwind (Vite build)   │
└───────────────┬───────────────────┘
                │ HTTP (JSON API)
┌───────────────▼───────────────────┐
│        Go HTTP Server             │
│                                   │
│   /              → embedded SPA   │
│   /api/...       → JSON handlers  │
│   /api/ws        → WebSocket      │
│                                   │
│   ncruces/go-sqlite3 (pure Go)   │
│   Single binary, zero CGO         │
└───────────────────────────────────┘
```

On launch, the Go server:

1. Opens (or creates) the SQLite database
2. Runs migrations
3. Starts listening on `odin.localhost:8080`
4. Opens the user's default browser to that address
5. Serves the embedded React app for all non-API routes
6. Shuts down cleanly on `Ctrl+C` or when the browser tab closes (via
   WebSocket heartbeat)

The `.localhost` domain resolves to `127.0.0.1` automatically in modern
browsers per [RFC 6761](https://datatracker.ietf.org/doc/html/rfc6761#section-6.3)
— no `/etc/hosts` edit needed.

---

## Backend Layers

```
┌──────────────────────────────────────────────────┐
│              HTTP Router (net/http)               │
│         JSON request/response, middleware         │
├──────────────────────────────────────────────────┤
│               handler layer                       │
│  internal/server/handlers/*.go                    │
│  Validates input. Calls service. Returns JSON.    │
├──────────────────────────────────────────────────┤
│                service layer                      │
│  internal/module/*/service.go                     │
│  Business rules, cross-module coordination.       │
├──────────────────────────────────────────────────┤
│               repository layer                    │
│  internal/module/*/repository.go                  │
│  Raw SQL queries. No business logic.              │
├──────────────────────────────────────────────────┤
│               database layer                      │
│  internal/database/db.go                          │
│  Connection, WAL mode, migrations, embed.FS.      │
├──────────────────────────────────────────────────┤
│         SQLite (ncruces/go-sqlite3)               │
│              Pure Go, zero CGO                    │
└──────────────────────────────────────────────────┘
```

**`internal/server/handlers/`** replaces the old Wails `bindings/` layer. Same
role — thin wrappers that validate input, call services, and format output —
but now as standard HTTP handlers returning JSON instead of Wails IPC structs.

**Module-per-directory**: Each compliance module is self-contained with its own
repository, service, models, reports, and migration SQL. Adding a future module
means adding a directory and registering routes.

### API Design

RESTful JSON API. The React frontend calls these via `fetch`:

```
GET    /api/establishments
POST   /api/establishments

GET    /api/incidents?establishment_id=1&status=open
POST   /api/incidents
GET    /api/incidents/:id
PUT    /api/incidents/:id
DELETE /api/incidents/:id

GET    /api/chemicals?establishment_id=1
GET    /api/chemicals/tier2?establishment_id=1

GET    /api/training/matrix?establishment_id=1
GET    /api/training/gaps?establishment_id=1

GET    /api/schema/tables?establishment_id=1
POST   /api/schema/tables
GET    /api/schema/tables/:id/records
POST   /api/schema/tables/:id/records

GET    /api/reports
GET    /api/reports/:id/run?establishment_id=1&year=2026
GET    /api/reports/:id/export?format=csv
```

### Database Access

Raw SQL via `database/sql` with `ncruces/go-sqlite3`. No ORM. The existing
schemas have 132 tables with complex views, triggers, and regulatory-specific
queries that map poorly to ORM patterns.

WAL mode allows the frontend to read while background tasks (audit logging,
report generation) write.

### Migration Strategy

Ordered migrations embedded from `embed/migrations/*.sql`:

1. Core (000) — establishments, employees, settings, audit_log
2. Modules by number — incidents (001), chemicals (002), training (003)
3. Schema Builder (100) — runs last

Version tracking in `schema_version` table. Each migration runs in a
transaction. Reference data seeded after structural migrations.

### Audit Logging

Every service that modifies data takes an `audit.Logger`. Called from the
service layer, not the repository — the service decides what constitutes an
auditable action.

### Error Handling

Typed errors in the service layer. HTTP handlers map them to status codes:

| Error              | HTTP Status |
| ------------------ | ----------- |
| `ErrNotFound`      | 404         |
| `ErrInvalidInput`  | 400         |
| `ErrDuplicateCase` | 409         |
| _(unexpected)_     | 500         |

The React frontend handles errors in the API client and surfaces them via toast
notifications.

---

## Frontend Architecture

### Routing

React Router with client-side routing. The Go server returns the SPA for all
non-`/api/` routes.

```
/                     → Dashboard
/setup                → First-run wizard
/incidents            → IncidentList
/incidents/:id        → IncidentDetail
/incidents/new        → IncidentForm
/chemicals            → ChemicalList
/chemicals/tier2      → Tier II report
/training             → TrainingMatrix
/training/gaps        → GapAnalysis
/schema               → TableList (custom tables)
/schema/:id/design    → TableDesigner
/employees            → EmployeeList
/reports              → Report browser
/settings             → Settings
```

### State Management

React context + hooks for global state. `useEstablishment()` is the most
critical hook — nearly every API call filters by establishment. Changing it
triggers refetches across visible components.

Page-level state stays in page components (filter/sort/pagination on lists,
draft state on forms).

### Component Patterns

All pre-built modules follow **List → Detail → Form**:

- **List pages**: `DataTable` with module-specific column configs
- **Detail pages**: Read-only view with related data sections
- **Form pages**: React Hook Form with validation
- **Schema Builder forms**: Dynamic `RecordForm` that renders from field
  metadata at runtime

### Real-Time Updates

WebSocket connection for server → client notifications:

- `establishment:changed` — facility switched
- `module:data-changed` — `{module, action, id}`
- `backup:completed` — after automatic backup

Receiving components refetch from the API on relevant events.

---

## Multi-Establishment Support

Every table with user data has `establishment_id`. The repository layer always
requires it as a parameter — no implicit "current establishment" at the data
layer. That concept lives in the React context and flows through API calls.

Custom tables created by the Schema Builder get `establishment_id`
automatically.

---

## Reporting Pipeline

### Report Registry

Reports are named queries mapping to SQL views or parameterized SQL:

| Report                       | Module    |
| ---------------------------- | --------- |
| OSHA 300 Log                 | incidents |
| OSHA 300A Summary            | incidents |
| Current Chemical Inventory   | chemicals |
| Tier II Reportable Chemicals | chemicals |
| TRI Reportable Chemicals     | chemicals |
| SDS Review Status            | chemicals |
| Training Gap Analysis        | training  |
| Employee Training Matrix     | training  |

### Export

- **CSV**: Pure Go serialization, served as download
- **PDF via Huginn**: Odin formats data as markdown + YAML frontmatter, sends to
  Huginn for rendering. If Huginn unavailable, PDF button disabled, CSV offered.
- **Custom tables**: Reportable via the same engine using Schema Builder's query
  builder.

---

## First-Run Experience

When the database has no `current_establishment_id`, the app routes to `/setup`:

1. **Welcome** — intro, detect ecosystem tools
2. **Create Establishment** — name, address, NAICS code
3. **Choose Modules** — incidents, chemicals, training (checkboxes)
4. **Quick Start** — import employees from CSV, add first chemical
5. **Dashboard** — ready to work

---

## Build and Distribution

```bash
# Build frontend
cd frontend && npm run build

# Build single binary (embeds frontend + migrations)
go build -o odin ./cmd/odin
```

No CGO. Trivial cross-compilation:

```bash
GOOS=linux   GOARCH=amd64 go build -o odin-linux   ./cmd/odin
GOOS=darwin  GOARCH=arm64 go build -o odin-darwin   ./cmd/odin
GOOS=windows GOARCH=amd64 go build -o odin.exe      ./cmd/odin
```

Database location follows platform conventions:

| Platform | Path                                   |
| -------- | -------------------------------------- |
| Linux    | `~/.local/share/odin/odin.db`          |
| macOS    | `~/Library/Application Support/odin/`  |
| Windows  | `%APPDATA%\odin\odin.db`              |

---

## Architecture Decision Records

**ADR-1: Embedded HTTP server + system browser, not Wails** — eliminates CGO
entirely. Full React/Tailwind UI with zero native dependencies. Trivial
cross-compilation. The compliance tool doesn't need native window chrome.

**ADR-2: `ncruces/go-sqlite3`, not `mattn/go-sqlite3`** — pure Go SQLite via
WASM. Full SQLite compatibility (WAL, FTS5, triggers, views) with zero CGO.

**ADR-3: React, not Svelte** — larger ecosystem for data-heavy admin UIs.
shadcn/ui, TanStack Table, React Hook Form are battle-tested for the exact UI
patterns Odin needs (tables, forms, reports).

**ADR-4: Raw SQL, not ORM** — 132 tables with complex views, triggers, and
regulatory queries. ORM would fight these.

**ADR-5: Schema Builder uses metadata tables** — enables UI generation,
validation before DDL, generic reporting over custom tables, and schema
versioning.

**ADR-6: Single SQLite database** — FKs work across modules, custom tables can
relate to pre-built tables, unified audit log, single-file backup.

**ADR-7: `cx_` prefix for custom tables** — prevents collisions with pre-built
module tables. Visible in any SQLite browser.

**ADR-8: `odin.localhost` domain** — modern browsers resolve `*.localhost` to
loopback per RFC 6761. Clean address bar with no `/etc/hosts` edit needed.

**ADR-9: Corrective actions consolidation deferred** — MVP uses per-module
corrective actions. Unified polymorphic table planned for post-MVP
inspections/audits module.
