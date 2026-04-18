---
layout: docs
title: Requirements
permalink: /docs/muninn/requirements/
---

[Back to Muninn docs](/docs/muninn/)

# Requirements

To build Muninn from source:

- Go 1.26+
- Node.js + npm (optional — only needed to build the VS Code extension)

No C compiler, CGO, or external libraries required.

## Building

```bash
git clone https://github.com/asgardehs/muninn.git
cd muninn
go build -o muninn ./cmd/muninn
```

After building, add the binary to your `PATH`:

**Linux:**

```bash
sudo cp muninn /usr/local/bin/
```

**macOS:**

```bash
cp muninn /usr/local/bin/
```

**Windows (PowerShell, run as Administrator):**

```powershell
New-Item -ItemType Directory -Force -Path "$env:LOCALAPPDATA\Programs\muninn"
Copy-Item muninn.exe "$env:LOCALAPPDATA\Programs\muninn\"

# Add to your user PATH (persistent across sessions)
$currentPath = [Environment]::GetEnvironmentVariable("Path", "User")
[Environment]::SetEnvironmentVariable("Path", "$currentPath;$env:LOCALAPPDATA\Programs\muninn", "User")
```

## VS Code Extension

The VS Code extension provides LSP-powered wikilink navigation and a "New Note"
command. Requires Node.js and npm.

```bash
cd vscode
npm install
npm run compile
npm run package
```

This produces `muninn-0.1.0.vsix`. Install it with:

```bash
code --install-extension muninn-0.1.0.vsix
```

Or use `muninn install` after building.
