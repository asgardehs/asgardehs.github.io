---
layout: default
title: Requirements
permalink: /docs/muninn/requirements/
---

[Back to Muninn docs](/docs/muninn/)

# Requirements

To build Muninn from source system requirements must be met:

- Go 1.26+
- A C compiler — CGO is required for SQLite, sqlite-vec, and ONNX Runtime
- ONNX Runtime shared libraries (`libonnxruntime.so` / `.dylib`)
- Node.js + npm (optional — only needed to build the VS Code extension)

| Platform | C Compiler                                | Notes                                          |
| -------- | ----------------------------------------- | ---------------------------------------------- |
| Linux    | `gcc` (usually pre-installed)             | Install via your package manager if missing    |
| macOS    | Xcode Command Line Tools                  | Run `xcode-select --install` if needed         |
| Windows  | [MSYS2](https://www.msys2.org/) MinGW-w64 | Install, then `pacman -S mingw-w64-x86_64-gcc` |

## Building

### Linux / macOS:

```bash
git clone https://github.com/asgardehs/muninn.git
cd muninn
CGO_ENABLED=1 go build -tags fts5 -o muninn ./cmd/muninn
```

### Windows

**USE Powershell, not Command Prompt**

```powershell
git clone https://github.com/asgardehs/muninn.git
cd muninn
$env:CGO_ENABLED=1
go build -tags fts5 -o muninn.exe ./cmd/muninn
```

> The `fts5` build tag enables full-text search (exact string matching)
> alongside semantic search. Without it, Muninn still works — semantic search is
> the primary search method — but exact-match search won't be available.
> **Windows note:** Make sure the MSYS2 MinGW `bin/` directory is in your `PATH`
> so `go` can find `gcc`. Typically: `C:\msys64\mingw64\bin`

After successful build add the binary to your path:

**Linux:**

```bash
sudo cp muninn /usr/local/bin/
```

**macOS:**

```bash
cp muninn /usr/local/bin/
# or if you prefer Homebrew's location:
cp muninn "$(brew --prefix)/bin/"
```

**Windows (PowerShell, run as Administrator):**

```powershell
# Create a directory for your tools if you don't have one
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

## Registration

After building, register Muninn with Claude Code and VS Code:

```bash
muninn install
```

This writes the daemon URL to Claude Code's config and installs the VS Code
extension. To target a specific tool:

```bash
muninn install --code    # Claude Code only
muninn install --vscode  # VS Code extension only
```

> **Note:** Claude Desktop's chat interface does not yet support the HTTP MCP
> transport that the Muninn daemon uses. Use Claude Code (via the desktop app or
> terminal) to access Muninn's tools.
