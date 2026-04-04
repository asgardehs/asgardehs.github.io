---
layout: default
---

<style>
.tool-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin: 30px 0;
}
.tool-card {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid #333;
  border-radius: 6px;
  padding: 24px;
  transition: border-color 0.2s;
}
.tool-card:hover {
  border-color: #e8e8e8;
}
.tool-card h3 {
  margin-top: 0;
  margin-bottom: 6px;
}
.tool-card h3 a {
  text-decoration: none;
}
.tool-card .badge {
  display: inline-block;
  font-size: 0.7em;
  padding: 2px 8px;
  border-radius: 3px;
  vertical-align: middle;
  margin-left: 6px;
  font-weight: normal;
}
.badge-active { background: #238636; color: #fff; }
.badge-dev { background: #8957e5; color: #fff; }
.tool-card p {
  font-size: 0.95em;
  color: #999;
  margin-bottom: 10px;
}
.tool-card .tech {
  font-size: 0.8em;
  color: #666;
}
.principles {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 16px;
  margin: 20px 0 30px;
}
.principle {
  text-align: center;
  padding: 16px 10px;
}
.principle strong {
  display: block;
  margin-bottom: 4px;
  color: #e8e8e8;
}
.principle span {
  font-size: 0.85em;
  color: #999;
}
.architecture {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid #333;
  border-radius: 6px;
  padding: 24px;
  margin: 30px 0;
  text-align: center;
}
.architecture pre {
  display: inline-block;
  text-align: left;
  font-size: 0.85em;
  line-height: 1.5;
  border: none;
  background: none;
  padding: 0;
}
@media (max-width: 720px) {
  .tool-grid { grid-template-columns: 1fr; }
  .principles { grid-template-columns: 1fr; }
}
</style>

Open-source compliance tools built for EHS professionals who need reliable, local-first software. Each tool ships as a single Go binary with zero runtime dependencies --- use them standalone or connect them through Bifrost for a unified workflow.

<div class="principles">
  <div class="principle">
    <strong>Local-First</strong>
    <span>Your data stays on your machine. No cloud accounts, no subscriptions.</span>
  </div>
  <div class="principle">
    <strong>Zero Dependencies</strong>
    <span>Single binaries. Download, run, done.</span>
  </div>
  <div class="principle">
    <strong>Graceful Degradation</strong>
    <span>Every tool works alone. Integration is a bonus, not a requirement.</span>
  </div>
</div>

---

## Tools

<div class="tool-grid">
  <div class="tool-card">
    <h3><a href="https://github.com/pharomwinters/muninn">Muninn</a> <span class="badge badge-active">active</span></h3>
    <p>Knowledge base and semantic search. Capture notes, snippets, and documentation from your terminal, editor, or AI assistant.</p>
    <span class="tech">CLI &middot; MCP Server &middot; LSP / VS Code Extension &middot; SQLite</span>
  </div>
  <div class="tool-card">
    <h3><a href="https://github.com/pharomwinters/huginn">Huginn</a> <span class="badge badge-dev">in development</span></h3>
    <p>Markdown-to-PDF document generation with a component system. Professional theming, automatic pagination, and a grid layout engine.</p>
    <span class="tech">Goldmark &middot; Maroto v2 &middot; YAML Themes</span>
  </div>
  <div class="tool-card">
    <h3><a href="https://github.com/pharomwinters/odin">Odin</a> <span class="badge badge-dev">in development</span></h3>
    <p>Web-based compliance management. Facility inspections, regulatory tracking, and a unified dashboard for the entire ecosystem.</p>
    <span class="tech">Web UI &middot; Facility Management &middot; Inspection Tracking</span>
  </div>
  <div class="tool-card">
    <h3><a href="https://github.com/pharomwinters/bifrost">Bifrost</a> <span class="badge badge-dev">in development</span></h3>
    <p>The bridge connecting ecosystem tools. Service discovery, message routing, health monitoring, and unified configuration via Heimdall.</p>
    <span class="tech">IPC &middot; Service Discovery &middot; Config Management</span>
  </div>
</div>

---

## Architecture

<div class="architecture">
<pre>
┌──────────┐     ┌─────────────────┐     ┌──────────┐
│   Odin   │◄───►│    Bifrost      │◄───►│  Muninn  │
│  (web)   │     │  ┌───────────┐  │     │  (kb)    │
└──────────┘     │  │ Heimdall  │  │     └──────────┘
                 │  │ (config)  │  │
                 │  └───────────┘  │
                 └────────┬────────┘
                          │
                    ┌─────▼─────┐
                    │  Huginn   │
                    │  (pdf)    │
                    └───────────┘
</pre>
</div>

Each tool communicates through **Bifrost** using local IPC. **Heimdall**, Bifrost's built-in configuration manager, provides a single source of truth for settings across all tools --- accessible through Odin's web UI, CLI commands, or MCP.

When Bifrost isn't running, every tool falls back to its own local configuration and continues working independently.

---

## Getting Started

Muninn is the first tool ready for use. Install it and start capturing knowledge:

```bash
go install github.com/pharomwinters/muninn@latest
```

See the [Muninn documentation](https://github.com/pharomwinters/muninn/wiki) for setup and usage.

---

## License

All Asgard tools are released under the [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html).
