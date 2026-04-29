---
layout: docs
title: Odin Documentation
permalink: /docs/odin/
---

# Odin

_The all-seeing overseer — compliance management for manufacturing._

Odin is a desktop EHS/compliance application targeting small manufacturing
facilities that carry the same regulatory burden as large enterprises but lack
the IT budget for enterprise ERP modules. It ships as a single binary per
platform with an embedded SQLite database — no server, no cloud, no IT
department.

## Technical Stack

| Layer        | Technology                                |
| ------------ | ----------------------------------------- |
| Backend      | Go HTTP server                            |
| Frontend     | React + Tailwind CSS + Vite               |
| Database     | SQLite (ncruces/go-sqlite3, pure Go)      |
| UI           | System browser (`odin.localhost`)         |
| Distribution | Single binary via `go:embed`, zero CGO    |

## MVP Modules

| Module          | Regulatory Coverage                                             |
| --------------- | --------------------------------------------------------------- |
| Incidents       | OSHA 300, 300A, 301                                             |
| Chemicals / SDS | OSHA HazCom, EPA Tier II (EPCRA 311/312), SARA 313/TRI         |
| Training        | Multi-regulatory (HazCom, Forklift, LOTO, Confined Space, etc.) |
| Schema Builder  | User-defined tables, fields, and relationships                  |

## Design Documentation

- [Architecture](/docs/odin/architecture/) — backend layers, frontend patterns,
  data flow
- [Schema Builder](/docs/odin/schema-builder/) — custom table engine
- [Database Design](/docs/odin/database/) — schema summary across all modules
- [Integration](/docs/odin/integration/) — optional Heimdall configuration
