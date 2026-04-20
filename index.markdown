---
layout: landing
title: Asgard EHS
description: Open-source, local-first EHS compliance tools. Single-binary Go applications that work standalone or as an integrated ecosystem.
---

<section class="hero">
  <div class="hero-inner">
    <p class="hero-eyebrow">Open-source &middot; Local-first &middot; GPL-3.0</p>
    <h1>Compliance tools that live on your machine.</h1>
    <p class="hero-sub">Single-binary Go applications for Environmental Health &amp; Safety professionals. Use them standalone, or wire them together through shared configuration.</p>
    <div class="hero-ctas">
      <a class="btn btn-primary" href="{{ '/docs/muninn/' | relative_url }}">Read the docs</a>
      <a class="btn btn-secondary" href="https://github.com/asgardehs" rel="noopener">View on GitHub</a>
    </div>
  </div>
</section>

<section class="section section-alt">
  <div class="section-inner">
    <header class="section-header">
      <span class="section-eyebrow">Principles</span>
      <h2>Built on three ideas.</h2>
      <p class="section-lede">Reliability over novelty. The tools you trust are the ones that get out of your way.</p>
    </header>
    <div class="principles">
      <div class="principle">
        <span class="principle-num">01</span>
        <h3>Local-first</h3>
        <p>Your data stays on your machine. No cloud accounts, no subscriptions, no telemetry.</p>
      </div>
      <div class="principle">
        <span class="principle-num">02</span>
        <h3>Single-binary Go or Rust</h3>
        <p>Download, run, done. No runtime to install, no package manager to wrangle.</p>
      </div>
      <div class="principle">
        <span class="principle-num">03</span>
        <h3>Graceful degradation</h3>
        <p>Every tool works alone. Integration is a bonus, not a requirement.</p>
      </div>
    </div>
  </div>
</section>

<section class="section">
  <div class="section-inner">
    <header class="section-header">
      <span class="section-eyebrow">The ecosystem</span>
      <h2>Five tools. One architecture.</h2>
      <p class="section-lede">Each named for a figure in Norse myth, each built to do one job well.</p>
    </header>
    <div class="tools">
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/muninn">Muninn</a></h3>
          <span class="status status-active">Active</span>
        </div>
        <p>Knowledge base and note management. Flat-file markdown vault with text search and wikilinks. Edit in any editor, search from the terminal.</p>
        <div class="tool-tech">CLI &middot; LSP / VS Code &middot; Flat files</div>
        <div class="tool-links">
          <a href="{{ '/docs/muninn/' | relative_url }}">Documentation</a>
          <a href="{{ '/docs/muninn/quickstart/' | relative_url }}">Quick start</a>
        </div>
      </article>
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/heimdall">Heimdall</a></h3>
          <span class="status status-active">Active</span>
        </div>
        <p>Shared configuration library for the ecosystem. Schema-validated settings, change notifications, cross-platform paths. Imported directly by each tool.</p>
        <div class="tool-tech">Go library &middot; CLI &middot; SQLite &middot; Schema validation</div>
        <div class="tool-links">
          <a href="{{ '/docs/heimdall/' | relative_url }}">Documentation</a>
          <a href="{{ '/docs/heimdall/quickstart/' | relative_url }}">Quick start</a>
        </div>
      </article>
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/ratatoskr">Ratatoskr</a></h3>
          <span class="status status-active">Active</span>
        </div>
        <p>Embedded Python distribution for Go applications. Ships the interpreter inside the binary and invokes it via subprocess — no CGO, no user installs, straightforward cross-compilation.</p>
        <div class="tool-tech">Go library &middot; python-build-standalone &middot; Apache-2.0</div>
        <div class="tool-links">
          <a href="{{ '/docs/ratatoskr/' | relative_url }}">Documentation</a>
          <a href="{{ '/docs/ratatoskr/quickstart/' | relative_url }}">Quick start</a>
        </div>
      </article>
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/huginn">Huginn</a></h3>
          <span class="status status-dev">In development</span>
        </div>
        <p>Markdown-to-PDF document generation with a component system. Professional theming, automatic pagination, and a grid layout engine.</p>
        <div class="tool-tech">Goldmark &middot; Maroto v2 &middot; YAML themes</div>
      </article>
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/odin">Odin</a></h3>
          <span class="status status-dev">In development</span>
        </div>
        <p>Desktop compliance management for manufacturing. Incidents, chemicals, training, and a schema builder for custom data — all in a single binary.</p>
        <div class="tool-tech">Go + React &middot; SQLite &middot; Zero CGO &middot; OSHA / EPA / RCRA</div>
        <div class="tool-links">
          <a href="{{ '/docs/odin/' | relative_url }}">Documentation</a>
          <a href="{{ '/docs/odin/architecture/' | relative_url }}">Architecture</a>
        </div>
      </article>
    </div>
  </div>
</section>

<section class="section section-alt">
  <div class="section-inner">
    <header class="section-header">
      <span class="section-eyebrow">Architecture</span>
      <h2>Standalone by default. Integrated by choice.</h2>
    </header>
<div class="prose-column" markdown="1">

No daemons. No IPC. No message bus. Each tool is a Go binary you run on its own.

**Heimdall** is a shared Go library that provides a single source of truth for configuration across all tools, with schema validation and change notifications. Tools also share vault directories and databases on disk.

Every tool works independently. Shared configuration is a convenience, not a dependency.

</div>
  </div>
</section>

<section class="section">
  <div class="section-inner">
    <header class="section-header">
      <span class="section-eyebrow">Getting started</span>
      <h2>Install Muninn in one command.</h2>
      <p class="section-lede">Muninn is the first tool ready for use. Install it and start capturing knowledge.</p>
    </header>
    <pre class="install-cmd"><code>go install github.com/asgardehs/muninn@latest</code></pre>
    <p class="section-footnote">See the <a href="{{ '/docs/muninn/' | relative_url }}">Muninn documentation</a> for setup and usage.</p>
  </div>
</section>

<section class="section section-alt">
  <div class="section-inner">
    <div class="cta-block">
      <h2>Built by an EHS professional.</h2>
      <p>Asgard is built by an EHS professional with 15+ years in automotive manufacturing who learned to code to solve the problems he kept running into on the job.</p>
      <div class="hero-ctas">
        <a class="btn btn-primary" href="{{ '/developer/' | relative_url }}">Read more</a>
        <a class="btn btn-secondary" href="{{ '/about/' | relative_url }}">About the project</a>
      </div>
    </div>
  </div>
</section>
