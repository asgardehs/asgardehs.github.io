---
layout: page
title: Changelog
description: Material changes to the Asgard EHS project and documentation site.
permalink: /changelog/
---

Notable changes to the Asgard EHS project and documentation site. Format
loosely follows [Keep a Changelog](https://keepachangelog.com/); dates are
ISO-8601.

## 2026-04-29 — Security policy published

Published the project's [security policy](/security/) (version 0.1.0).
The policy follows the [OWASP Vulnerability Disclosure Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Vulnerability_Disclosure_Cheat_Sheet.html)
and documents the disclosure process, expected response times, scope,
and the project's PGP public key for encrypting sensitive reports. The
policy honestly names the solo-maintainer constraint and frames its
timing targets as commitments, not guarantees. Linked from the site
footer and the developer page.

Also corrected three stale stack references on the landing page and the
schema-builder doc that still described the old Wails+Svelte
implementation; the architecture ADRs had already moved to embedded HTTP
server + React.

## 2026-04-28 — Scope reduction to Odin

The project narrowed its public scope to a single primary application
(Odin) plus the two libraries that support it (Heimdall, Ratatoskr). The
decision was made deliberately, in conversation with family and a mentor,
to keep the maintainer's effort focused on the part of the ecosystem that
delivers value to EHS professionals end-to-end.

**Removed from the site:**

- Muninn (knowledge base) — repository removed from GitHub.
- Huginn (PDF generation) — deprioritized; not currently planned.

**Repositioned:**

- Heimdall and Ratatoskr framed as supporting libraries for Odin (and
  available standalone), not peer applications.
- Odin is the only end-user product on the site.
- Mimir (ontology viewer) and EHS-Ontology (Odin's data model) are
  treated as Odin sub-projects and intentionally not listed on the doc
  site at this time.

**Other changes:**

- Brand guidelines license section now covers three licenses:
  Apache-2.0 for libraries, GPL-3.0 for end-user applications, and
  CC BY-SA 4.0 for creative works (ontologies, research notes).
- Trademark and usage examples in the brand guidelines were genericized
  to remove specific product names where possible, so the guidelines
  remain valid as the ecosystem changes.
- Project email changed from `muninn.developer@protonmail.com` to
  `asgardehs@proton.me`.
- Developer page now publishes the PGP fingerprint used to sign commits
  across the Asgard EHS repositories. A formal security policy that will
  reference this key is in progress.
- This changelog was added and linked from the site footer and brand
  guidelines, fulfilling the brand doc's existing promise that material
  changes would be recorded publicly.
