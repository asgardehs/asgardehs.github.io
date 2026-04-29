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
      <a class="btn btn-primary" href="{{ '/docs/odin/' | relative_url }}">Read the docs</a>
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
      <h2>One application. Two supporting libraries.</h2>
      <p class="section-lede">Odin is the desktop application. Heimdall and Ratatoskr are the libraries that power it — and stand on their own as open-source Go packages.</p>
    </header>
    <div class="tools">
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/odin">Odin</a></h3>
          <span class="status status-dev">In development</span>
        </div>
        <p>Desktop compliance management for manufacturing. Incidents, chemicals, training, and a schema builder for custom data — all in a single binary.</p>
        <div class="tool-tech">Wails v2 + Svelte &middot; SQLite &middot; OSHA / EPA / RCRA</div>
        <div class="tool-links">
          <a href="{{ '/docs/odin/' | relative_url }}">Documentation</a>
          <a href="{{ '/docs/odin/architecture/' | relative_url }}">Architecture</a>
        </div>
      </article>
      <article class="tool">
        <div class="tool-head">
          <h3><a href="https://github.com/asgardehs/heimdall">Heimdall</a></h3>
          <span class="status status-active">Active</span>
        </div>
        <p>Shared configuration library. Schema-validated settings, change notifications, cross-platform paths. Used by Odin and available as a standalone Go package.</p>
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

No daemons. No IPC. No message bus. Odin is a Go binary you run on its own; Heimdall and Ratatoskr are libraries you import.

**Heimdall** provides Odin's single source of truth for configuration, with schema validation and change notifications. **Ratatoskr** ships the Python interpreter inside the binary so analysis features work without a separate install.

Each library stands on its own. They power Odin, but you can pull either into your own Go project — they're not coupled to the application.

</div>
  </div>
</section>

<section class="section">
  <div class="section-inner">
    <header class="section-header">
      <span class="section-eyebrow">Getting started</span>
      <h2>Use the libraries today. Watch Odin take shape.</h2>
      <p class="section-lede">Heimdall and Ratatoskr are available as Go modules. Odin is in active development — follow along on GitHub.</p>
    </header>
    <pre class="install-cmd"><code>go get github.com/asgardehs/heimdall@latest</code></pre>
    <p class="section-footnote">See the <a href="{{ '/docs/heimdall/' | relative_url }}">Heimdall documentation</a> to get started, or read the <a href="{{ '/docs/odin/' | relative_url }}">Odin docs</a> for what's coming.</p>
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
