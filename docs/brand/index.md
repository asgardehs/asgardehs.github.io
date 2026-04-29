---
layout: docs
title: Brand Guidelines
description: Identity, naming, visual system, and voice for the Asgard EHS project.
permalink: /docs/brand/
---

This document is the single reference for how Asgard EHS presents itself — to
itself, to contributors, and to the world. It is a companion to the
[Nótt & Dagr theme specification](/docs/theme/), which defines the technical
design tokens. The theme spec answers "what is the hex value for Cyan?" This
document answers "when should something be Cyan?"

It is aimed at three audiences, in descending order of priority: the
maintainer (keeping the project consistent over time), contributors (matching
the project's voice and visual system in PRs and new documentation), and third
parties (bloggers, tutorial authors, package maintainers, forks).

## The Ecosystem

**Asgard EHS** is an ecosystem of open-source tools for environmental health
and safety professionals. The full name "Asgard EHS" is the canonical
reference. "Asgard" is acceptable as a short form after the full name has been
established earlier in the same document, conversation, or webpage.

The tools in the ecosystem are:

| Tool           | Role                                              | Status          |
| -------------- | ------------------------------------------------- | --------------- |
| **Heimdall**   | Shared configuration library                      | Active          |
| **Ratatoskr**  | Embedded Python distribution for Go applications  | Active          |
| **Odin**       | Desktop compliance management for manufacturing   | In development  |

Each tool is a proper noun and is always capitalized when referring to the
product. The lowercase form (`heimdall`, `odin`, etc.) is used only when
referring to the binary or the command. "I ran `heimdall config`" is correct;
"odin is a compliance tool" is not.

### Naming Rules for Future Tools

Every tool in Asgard EHS takes its name from Norse mythology, and the name
must match the function. Muninn is the raven of memory, and the tool is a
memory system. Heimdall is the watchman of the gods, and the tool guards
shared state. This is not decoration — it is a constraint on the system.

When adding a new tool to the ecosystem:

1. Identify the tool's primary function before naming it.
2. Find a Norse figure, object, or concept whose traditional role maps to
   that function.
3. Prefer less-famous names (Muninn, Heimdall) over the most famous ones
   (Thor, Loki). A strong function match can justify a famous name; weak
   matches cannot.
4. If no Norse name fits, the tool probably doesn't belong in the ecosystem.

Bifrost, the rainbow bridge connecting Asgard to Midgard, is reserved for a
future integration or gateway tool. Other Norse elements — Yggdrasil (the
world tree), the Well of Urðr, Mímir — remain available and should be
matched to function rather than chosen for flavor.

### Thematic Groupings

**Ratatoskr** is the ecosystem's current messenger figure. In the mythology,
the squirrel of Yggdrasil runs the length of the world tree, carrying
messages between the eagle at the crown and the dragon Níðhöggr at the
roots. The library carries calls and replies between a Go process and the
embedded Python interpreter — the same role, expressed in software. This
match between a figure's traditional role and a tool's function is the kind
of fit future names should aim for. If a future tool is primarily about
carrying, shuttling, or relaying — logs, events, inter-process messages —
a messenger creature is the right family of names to draw from.

## Logo and Wordmark

The Asgard EHS mark is a single logo file located at
`assets/img/asgard-ehs-logo.png`. It is used in the site header, the 404
page, and anywhere the project represents itself.

### Clear Space

Maintain clear space around the logo equal to at least half the logo's
height on all sides. Nothing — text, other images, UI controls — should
encroach on that space.

### Minimum Size

The logo should render at a minimum height of 24 pixels on digital surfaces
and should never be reproduced smaller. At its current site implementation,
the header uses 36px (2.25rem) on desktop and 30px (1.85rem) on mobile.

### Backgrounds

The logo is designed to work on the Asgard backgrounds:

- **Dagr (light):** `#F5F1E4` and the `bg-light` / `surface` variants.
- **Nótt (dark):** `#1A1F2E` and the `bg-light` / `surface` variants.

Avoid placing the logo on saturated colors, photographs, or patterned
backgrounds without testing contrast first. When in doubt, place it on a
solid background that matches the current theme.

### What Not to Do

- Do not stretch, skew, rotate, or distort the logo.
- Do not recolor the logo. If a single-color treatment is ever needed, it
  must be generated from the source artwork, not the PNG.
- Do not add drop shadows, outlines, glows, or other effects.
- Do not place text directly against the logo without the clear-space margin.
- Do not combine the logo with other logos in a single lockup without the
  project maintainer's approval.

## Color System

Asgard EHS uses the [Nótt & Dagr](/docs/theme/) theme as both its technical
design system and its brand color system. The full palette — including syntax
colors, ANSI colors, UI surfaces, and functional colors — is documented in the
theme spec.

This section covers the handful of brand-level decisions that sit on top of
that spec.

### The Primary Accent is Cyan

Asgard EHS is a unified system, not a collection of independently branded
tools. The entire ecosystem shares a single accent color: **Cyan**.

- Nótt (dark): `#6EB8D6`
- Dagr (light): `#2B6A8A`

Cyan is the color of section eyebrows, link hover states, active sidebar
items, hero call-to-action emphasis, and any marketing or release material
that needs a single brand color. Individual tools do not own their own
colors. A Muninn release note and an Odin release note use the same palette;
only the logo and name distinguish them.

### Functional Colors Signal State

The Functional color set (defined in the theme spec) carries meaning, not
decoration. Use it consistently:

| Color              | Meaning                                | Example Use                        |
| ------------------ | -------------------------------------- | ---------------------------------- |
| Functional Green   | Success, active, stable                | "Active" status pill               |
| Functional Purple  | In development, focus, transitional    | "In development" status pill       |
| Functional Orange  | Warning, caution                       | Deprecation notices                |
| Functional Red     | Error, destructive, critical           | Breaking-change banners            |
| Functional Cyan    | Information, links, brand              | Primary CTAs, informational callouts |

Never use a functional color for decoration alone. If a button is green, it
should represent a success or active state, not a "this button is important"
signal. Use typography, placement, and size for emphasis.

### Status Pills

The ecosystem uses two status pills consistently across the site and
documentation:

- **Active** — Functional Green. The tool is usable and documented.
- **In development** — Functional Purple. The tool exists but is not yet
  ready for general use.

A third pill, **Planned**, may be added in the future for tools that are
designed but not yet implemented. Do not introduce new status pills without
updating this document.

## Typography

Asgard EHS uses three typefaces, each with a specific role. Do not introduce
additional fonts in project materials without a compelling reason and an
update to this section.

| Role    | Family           | Usage                                      |
| ------- | ---------------- | ------------------------------------------ |
| Display | Fraunces         | Headings, hero text, wordmarks             |
| Body    | Inter            | Paragraph text, UI labels, navigation      |
| Mono    | JetBrains Mono   | Code, commands, file paths, install blocks |

### Pairing Rules

- Headings are always Fraunces. Body text is always Inter. Code is always
  JetBrains Mono. These are the only three families used in project
  materials.
- Fraunces is used at its display optical size (`opsz` 144) for large
  headings and its default size for inline serif text.
- Avoid mixing faces within a single block of text. A paragraph is either
  all Inter or all Fraunces; a heading is either all Fraunces or all
  JetBrains Mono (for code-as-heading cases).

### Hierarchy

Follow the hierarchy established in the site stylesheet:

- Hero and landing headings use Fraunces at clamp ranges around 4–5.5rem.
- Section headings (h2) use Fraunces at clamp ranges around 1.7–3rem.
- Subsection headings (h3) use Fraunces at 1.35rem.
- Body text uses Inter at 1rem with a line-height of 1.65.
- Code blocks use JetBrains Mono at 0.92em relative to body text.

The specific values are defined in the theme CSS. When producing marketing
or documentation materials, follow the same proportional relationships even
if the absolute sizes differ.

## Voice and Tone

The Asgard EHS voice is **direct, technical, and understated**. The project
is built by an EHS professional who learned to code to solve real problems,
and the documentation sounds like it. Marketing polish that obscures
technical content is worse than plain text that delivers it.

### Principles

1. **Say what it does, not why you should care.** "Muninn is a flat-file
   knowledge base with text search and wikilinks" is better than "Muninn
   transforms how you capture and connect your ideas."
2. **Prefer concrete over abstract.** Reference real file paths, real
   commands, real regulatory standards (OSHA 300, EPA Tier II, ISO 45001)
   instead of generic language.
3. **No marketing superlatives.** Avoid "best," "amazing," "revolutionary,"
   "game-changing," "seamless," "cutting-edge," and similar. They are noise.
4. **Acknowledge limitations honestly.** If a feature is missing, say so.
   If a decision has trade-offs, name them. If something might not work in a
   given environment, warn about it.
5. **Do not talk down to readers.** The audience is EHS professionals and
   developers, both of whom work in domains with their own vocabulary.
   Explain acronyms on first use, then use them.
6. **Let the technical content carry the weight.** The project's
   credibility comes from the work, not from adjectives describing the work.

### Examples

**Do:**

> Odin is a desktop EHS/compliance application targeting small manufacturing
> facilities that carry the same regulatory burden as large enterprises but
> lack the IT budget for enterprise ERP modules.

**Don't:**

> Odin revolutionizes compliance management by seamlessly empowering
> manufacturers to effortlessly handle their regulatory needs.

**Do:**

> If Heimdall is not available, Odin falls back to its own settings table.
> The application works the same.

**Don't:**

> Heimdall integration unlocks a powerful new dimension of seamless
> cross-tool configuration.

**Do:**

> Heimdall and Ratatoskr are available as Go modules. Odin is in active
> development.

**Don't:**

> We're excited to announce that Ratatoskr has officially launched!

### Tone by Context

- **Documentation** — Neutral, instructional, example-heavy. Assume the
  reader has a task to accomplish and wants the shortest path to
  accomplishing it.
- **Release notes** — Factual. List what changed. Group by module or tool.
  Note breaking changes at the top. No celebration language.
- **README files** — A little more conversational than the docs, but still
  restrained. The reader is deciding whether to invest time in the project;
  give them the information to decide.
- **Commit messages** — Imperative mood ("Add incident recordability
  logic," not "Added" or "Adds"). Present tense. One-line summary under
  72 characters, followed by a blank line and a paragraph explaining the
  *why* if the change is not self-explanatory.
- **Code comments** — Explain why, not what. The code shows what; comments
  explain the reasoning, the trade-off, or the non-obvious constraint.

## Usage and Attribution

Asgard EHS uses three licenses, matched to what each component is:

- **Apache-2.0** covers **infrastructure libraries** intended to be imported
  by other projects. Permissive licensing removes adoption friction for
  downstream developers.
- **GPL-3.0** covers **end-user applications** distributed as runnable
  binaries. Strong copyleft preserves user protections and keeps
  compliance-critical functionality auditable.
- **CC BY-SA 4.0** covers **creative works** — ontologies, research notes,
  written guides, illustrations, and other content distinct from runnable
  software. The same share-alike spirit as GPL-3.0, applied to the medium
  that fits.

The rule is simple: if it is a library to import, it is Apache-2.0; if it is
an application to run, it is GPL-3.0; if it is a written or illustrative
work, it is CC BY-SA 4.0.

The project name, the individual tool names, and the logo are separate from
any code or content license. The sections below cover how those names and
marks may be used, and apply uniformly regardless of which license governs
the underlying work.

The project's posture is **permissive for legitimate use, protective against
confusion**. The goal is to ensure that when someone sees the Asgard EHS
name or any of its tool names attached to something, they can trust that it
is in fact that thing — or a close, compatible derivative.

### What Is Permitted Without Asking

- **Unmodified redistribution.** If you repackage an official Asgard EHS
  release unchanged — a Linux distribution packaging an application, a
  container image wrapping a binary, a mirror of the source code — you may
  use the project names. The artifact is what the user expects, and calling
  it by its name serves them.
- **Compatible forks that track upstream.** Forks that maintain
  compatibility with the official project and regularly merge from upstream
  may continue using the name. The test is whether a user of the fork would
  have the same experience as a user of the official version.
- **Editorial and informational use.** Articles, blog posts, tutorials,
  reviews, comparisons, and academic references may use the project names
  and logos freely. You do not need permission to write about Asgard EHS or
  any of its tools.
- **Compatibility statements.** Third-party products that work with an
  Asgard EHS tool may use phrasing like "Works with [tool name],"
  "Compatible with [tool name]," or "Powered by [tool name]." The
  third-party product's own branding must remain distinct; the statement
  must be factual.
- **Personal and internal use.** Any internal use within an organization —
  intranets, internal documentation, training materials — may reference the
  project names and logos without permission.

### What Requires Renaming

- **Materially divergent forks.** A fork that changes the target audience,
  breaks compatibility, alters the core feature set, or follows a
  substantially different roadmap must rename. A compliance tool forked
  into a general-purpose database builder is a different product and should
  have a different name.
- **Forks that do not track upstream.** A long-lived fork that no longer
  pulls from the official project has become its own project and must
  rename to avoid user confusion.
- **Derivative products with added commercial features.** A commercial
  distribution that bundles an Asgard EHS tool with proprietary extensions
  must use its own product name. "Acme Corp's [Tool] Pro" is not permitted;
  "Acme Corp's [Suite Name] (built on [Tool])" is.

### What Requires Permission

- **Commercial use of the logos.** Merchandise, paid training courses,
  hosted services marketed under the Asgard EHS name, or any product that
  charges money and prominently features the logo requires written
  permission from the project maintainer.
- **Implied endorsement.** Any usage that could reasonably be read as an
  official endorsement — "Officially recommended by Asgard EHS," "The
  Asgard EHS team uses X" — requires prior approval.
- **Domains and social media handles.** Registering domains or social
  accounts using the Asgard EHS name or tool names requires permission.
  This does not apply to subdomains or handles clearly marked as
  unofficial (e.g., `asgard-community.example.com`, `@asgard_fan`).

To request permission, email
[asgardehs@proton.me](mailto:asgardehs@proton.me)
with a description of the proposed use.

### Attribution

When referencing Asgard EHS in writing, the preferred attribution is:

> Asgard EHS — open-source compliance tooling for environmental health and
> safety professionals. Available at
> [github.com/asgardehs](https://github.com/asgardehs).

When referencing a specific tool, use its name and a brief description of
its function (see [The Ecosystem](#the-ecosystem) above), followed by the
repository URL.

---

*This document will evolve as the project grows. Material changes will be
recorded in the [project changelog](/changelog/) and announced in release
notes.*
