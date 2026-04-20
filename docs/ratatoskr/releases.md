---
layout: docs
title: Ratatoskr Releases & Platforms
permalink: /docs/ratatoskr/releases/
---

[Back to Ratatoskr docs](/docs/ratatoskr/)

# Releases & Platforms

## Supported Platforms

| OS      | Architecture   |
| ------- | -------------- |
| Linux   | amd64, arm64   |
| macOS   | amd64, arm64   |
| Windows | amd64          |

Platform support follows the upstream
[python-build-standalone](https://github.com/astral-sh/python-build-standalone)
distributions that Ratatoskr embeds. Adding a platform means adding it to the
release workflow matrix; it is not a code change.

## Versioning

Ratatoskr follows [semantic versioning](https://semver.org/) on its Go API.
Each release bundles a single Python version, called out in the release
notes — a given Ratatoskr release advertises "bundles Python 3.13.x" in its
description, and upgrading Ratatoskr may upgrade Python alongside it. Review
release notes before upgrading if your application depends on specific
Python interpreter behavior.

## Release Workflow

Releases are cut manually via the `release` GitHub Actions workflow
(`workflow_dispatch`). The maintainer supplies:

- The target version tag (e.g., `v1.2.0`)
- The Python version
- The
  [python-build-standalone](https://github.com/astral-sh/python-build-standalone)
  release date

The workflow generates the embedded distribution, runs the test suite on
Linux, macOS, and Windows, and pushes the resulting tag.
