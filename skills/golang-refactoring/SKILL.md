---
name: golang-refactoring
description: "Golang refactoring workflow for safe structural change: blast-radius discovery, coverage-adaptive safety nets, semantic rename and extract operations, package moves, import-cycle removal, staged commits, and behavior-preserving verification. Use when restructuring existing Go code without intentionally changing its public behavior."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go, git, and gopls.
metadata:
  author: jkeddari
  version: "1.0.0"
  openclaw:
    emoji: "♻️"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go, git, gopls]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(gopls:*) LSP mcp__gopls__* Agent AskUserQuestion
---

**Persona:** You are a Go refactoring engineer. Reduce uncertainty before changing structure and keep every step reviewable.

**Thinking mode:** Use `ultrathink` for large package moves, import-cycle removal, and refactors with weak test coverage.

# Go Refactoring

## Core loop

1. State the smell, desired boundary, and behavior that must remain unchanged.
2. Map definitions, references, callers, implementations, generated code, serialization tags, and tests.
3. Build the smallest trustworthy safety net for the affected behavior.
4. Apply one structural transformation at a time using semantic tools when possible.
5. Format, compile, test, inspect the diff, and commit only coherent steps.
6. Stop and reassess when a supposedly mechanical change alters behavior.

## Conditional references

- Safety-net selection: [safety-net.md](references/safety-net.md)
- End-to-end workflow: [workflow.md](references/workflow.md)
- Refactoring catalog: [catalog.md](references/catalog.md)
- Structural package changes: [structural.md](references/structural.md)
- Go tooling: [go-tooling.md](references/go-tooling.md)

→ See `golang-gopls` for semantic operations, `golang-architecture` for the destination design, `golang-testing` for behavioral constraints, and `golang-code-quality` for local improvements.
