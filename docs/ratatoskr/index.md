---
layout: docs
title: Ratatoskr Documentation
description: Embedded Python distribution for Go applications.
permalink: /docs/ratatoskr/
---

# Ratatoskr

Ratatoskr embeds a complete Python distribution inside a Go binary and
invokes it via subprocess. Go applications that need Python capabilities —
scientific libraries, regulatory calculation engines, or ecosystem-specific
tooling that only exists in Python — can ship the interpreter alongside the
binary rather than demanding that users install Python themselves or taking
on the fragility of CGO-based bindings.

It is a library first: import it, call it, and it handles extraction, path
management, and the subprocess lifecycle for you. Utilities are included for
embedding pip packages alongside the interpreter.

## Attribution

Ratatoskr is a fork of
[kluctl/go-embed-python](https://github.com/kluctl/go-embed-python), originally
created by the kluctl team and licensed under Apache-2.0. The upstream project
is no longer actively maintained. This fork continues the work under Asgard
EHS, preserving the original design while updating the library for continued
compatibility with new Python releases and supported platforms. All credit for
the original design and implementation belongs to the kluctl authors.

## When Not to Use Ratatoskr

Ratatoskr is a subprocess-based bridge, not an in-process Python runtime. It
is:

- **Not a CGO binding.** Python runs in a separate process. If you need
  in-process Python (direct memory sharing, zero-overhead calls, embedding
  Python objects as Go values), Ratatoskr is the wrong tool.
- **Not for hot-path performance.** Every Python invocation has subprocess
  startup and IPC overhead. Use Ratatoskr when the work done per call
  outweighs the call itself, not for tight inner loops.
- **Not a sandbox.** The embedded Python interpreter runs with the full
  permissions of the host process. Do not use Ratatoskr to execute
  untrusted Python code.

## Getting Started

- [Quick Start](/docs/ratatoskr/quickstart/) — install, first invocation, and
  persistent extraction for desktop apps
- [Embedding Libraries](/docs/ratatoskr/embedding/) — bundle pip packages
  alongside the interpreter
- [Releases & Platforms](/docs/ratatoskr/releases/) — supported platforms,
  Python versions, and the release workflow

## Project

- **License:** Apache-2.0
- **Source:** [github.com/asgardehs/ratatoskr](https://github.com/asgardehs/ratatoskr)
- **Code of Conduct:** see the project [Code of Conduct](/code-of-conduct/)
- **Security:** report vulnerabilities to
  [muninn.developer@protonmail.com](mailto:muninn.developer@protonmail.com)

## Name

> _In Norse mythology, Ratatoskr is the squirrel who runs the length of
> Yggdrasil, the world tree, carrying messages between the eagle at its
> crown and the dragon Níðhöggr at its roots. He is the messenger who
> crosses between realms that would otherwise never meet. Here, Ratatoskr
> carries calls and replies between the Go process and the Python runtime
> embedded within it._
