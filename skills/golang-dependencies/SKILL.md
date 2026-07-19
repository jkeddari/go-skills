---
name: golang-dependencies
description: "Golang module and dependency management using go.mod, go.sum, Minimal Version Selection, workspaces, vendoring, vulnerability checks, update automation, and conflict diagnosis. Use when adding, removing, upgrading, auditing, pinning, or troubleshooting Go dependencies and tool modules."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go and git.
metadata:
  author: jkeddari
  version: "0.1.0"
  openclaw:
    emoji: "📦"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go, git]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(govulncheck:*) Agent AskUserQuestion
---

# Go Dependencies

## Workflow

1. Inspect `go.mod`, `go.sum`, `go.work`, replace directives, and the repository policy before changing dependencies.
2. Ask before introducing a new production dependency unless the user already selected it.
3. Prefer the standard library when it meets the requirement without substantial reimplementation.
4. Make the smallest version change that solves the problem; do not batch unrelated upgrades.
5. Run `go mod tidy`, review both module files, then run focused tests and `go test ./...` when practical.
6. Treat vulnerability output as evidence to investigate, not an instruction to upgrade blindly.

## Conditional references

- Semantic import versioning and release policy: [versioning.md](references/versioning.md)
- MVS and conflict resolution: [conflicts.md](references/conflicts.md)
- Dependency audits: [auditing.md](references/auditing.md)
- Module graph inspection: [visualization.md](references/visualization.md)
- Workspaces and multi-module repositories: [workspaces.md](references/workspaces.md)
- Renovate or Dependabot policy: [automated-updates.md](references/automated-updates.md)

## Quick verification

```bash
go mod tidy
go mod verify
go list -m all
go test ./...
govulncheck ./...
```

→ See `golang-security` for exploitability analysis and `golang-project` for CI automation.
