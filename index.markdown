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

.site-nav {
  display: flex;
  gap: 24px;
  margin-bottom: 24px;
  padding-bottom: 12px;
  border-bottom: 1px solid #333;
  font-size: 0.95em;
}
.site-nav a {
  color: #999;
  text-decoration: none;
  transition: color 0.2s;
}
.site-nav a:hover {
  color: #e8e8e8;
}
.tool-card .links {
  margin-top: 12px;
  font-size: 0.85em;
}
.tool-card .links a {
  color: #999;
  text-decoration: none;
  margin-right: 16px;
}
.tool-card .links a:hover {
  color: #e8e8e8;
}
@media (max-width: 720px) {
  .tool-grid { grid-template-columns: 1fr; }
  .principles { grid-template-columns: 1fr; }
}
</style>

<div class="site-nav">
  <a href="/docs/odin/">Odin Docs</a>
  <a href="/docs/muninn/">Muninn Docs</a>
  <a href="/docs/heimdall/">Heimdall Docs</a>
  <a href="/about/">About</a>
  <a href="/developer/">Developer</a>
</div>

Open-source compliance tools built for EHS professionals who need reliable, local-first software. Each tool ships as a single Go binary with zero runtime dependencies --- use them standalone or together through shared configuration.

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
    <h3><a href="https://github.com/asgardehs/muninn">Muninn</a> <span class="badge badge-active">active</span></h3>
    <p>Knowledge base and note management. Flat-file markdown vault with text search and wikilinks. Edit in any editor, search from the terminal.</p>
    <span class="tech">CLI &middot; LSP / VS Code Extension &middot; Flat Files</span>
    <div class="links"><a href="/docs/muninn/">Documentation</a><a href="/docs/muninn/quickstart/">Quick Start</a></div>
  </div>
  <div class="tool-card">
    <h3><a href="https://github.com/asgardehs/heimdall">Heimdall</a> <span class="badge badge-active">active</span></h3>
    <p>Shared configuration library for the ecosystem. Schema-validated settings, change notifications, and cross-platform paths. Imported directly by each tool.</p>
    <span class="tech">Go Library &middot; CLI &middot; SQLite &middot; Schema Validation</span>
    <div class="links"><a href="/docs/heimdall/">Documentation</a><a href="/docs/heimdall/quickstart/">Quick Start</a></div>
  </div>
  <div class="tool-card">
    <h3><a href="https://github.com/asgardehs/huginn">Huginn</a> <span class="badge badge-dev">in development</span></h3>
    <p>Markdown-to-PDF document generation with a component system. Professional theming, automatic pagination, and a grid layout engine.</p>
    <span class="tech">Goldmark &middot; Maroto v2 &middot; YAML Themes</span>
  </div>
  <div class="tool-card">
    <h3><a href="https://github.com/asgardehs/odin">Odin</a> <span class="badge badge-dev">in development</span></h3>
    <p>Desktop compliance management for manufacturing. Incidents, chemicals, training, and a schema builder for custom data — all in a single binary.</p>
    <span class="tech">Go + React &middot; SQLite &middot; Zero CGO &middot; OSHA / EPA / RCRA</span>
    <div class="links"><a href="/docs/odin/">Documentation</a><a href="/docs/odin/architecture/">Architecture</a></div>
  </div>
</div>

---

## Architecture

<div class="architecture">
<pre>
┌──────────┐     ┌──────────┐     ┌──────────┐
│   Odin   │     │  Muninn  │     │  Huginn  │
│  (web)   │     │  (kb)    │     │  (pdf)   │
└────┬─────┘     └────┬─────┘     └────┬─────┘
     │                │                │
     └────────┬───────┴────────┬───────┘
              │                │
        ┌─────▼─────┐   ┌─────▼─────┐
        │ Heimdall  │   │  Shared   │
        │ (config)  │   │  Vault    │
        └───────────┘   └───────────┘
</pre>
</div>

Each tool is standalone --- no daemons, no IPC, no message bus. **Heimdall** is a shared Go library that provides a single source of truth for configuration across all tools, with schema validation and change notifications. Tools also share vault directories and databases on disk.

Every tool works independently. Shared configuration is a convenience, not a dependency.

---

## Getting Started

Muninn is the first tool ready for use. Install it and start capturing knowledge:

```bash
go install github.com/asgardehs/muninn@latest
```

See the [Muninn documentation](/docs/muninn/) for setup and usage.

---

## About the Developer

Asgard is built by an EHS professional with 15+ years in automotive manufacturing who learned to code to solve the problems he kept running into on the job. [Read more](/developer/).

---

## License

All Asgard tools are released under the [GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html).
