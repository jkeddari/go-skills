---
name: golang-gopls
description: "Golang semantic code intelligence with the official gopls language server: definitions, references, symbols, call and implementation hierarchies, diagnostics, formatting, code actions, safe rename, extract, inline, and package API discovery. Use when navigating or refactoring a local Go workspace or when semantic analysis is safer than text search."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go and gopls.
metadata:
  author: jkeddari
  version: "1.0.0"
  openclaw:
    emoji: "🧭"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go, gopls]
    install:
      - kind: go
        package: golang.org/x/tools/gopls@latest
        bins: [gopls]
    skill-library-version: "0.22.0"
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(gopls:*) LSP mcp__gopls__* Agent
---

# gopls Semantic Workflows

## Preference order

Use the available semantic interface in this order: gopls MCP tools, native LSP operations, then the `gopls` CLI. Fall back to text search only when semantic tooling is unavailable or the query is intentionally textual.

## Workflows

- Before a rename, find definitions, references, implementations, and interface satisfaction.
- Before moving code, inspect incoming and outgoing calls plus package dependencies.
- After edits, inspect diagnostics and run affected tests.
- Use code actions only after understanding the proposed transformation.
- Prefer package symbols and documentation over reading an entire dependency source tree.

## Conditional references

- Setup and transport options: [mcp.md](references/mcp.md)
- Workspace and build settings: [settings.md](references/settings.md)
- CLI commands: [cli.md](references/cli.md)
- Capability overview: [features.md](references/features.md)
- Interface selection matrix: [matrix.md](references/matrix.md)

→ See `golang-refactoring` for the staged change workflow and `golang-tooling` for lint and modernization.
