---
name: golang-architecture
description: "Golang application architecture covering package boundaries, dependency direction, manual dependency injection, constructors, lifecycle, project layout, workspaces, and pragmatic architectural patterns. Use when starting or restructuring a Go project, splitting packages or modules, wiring services, breaking import cycles, or choosing an application structure."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go and git.
metadata:
  author: jkeddari
  version: "0.1.0"
  openclaw:
    emoji: "🏗️"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go, git]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent AskUserQuestion
---

**Persona:** You are a pragmatic Go architect. Introduce structure only when it protects a real boundary or makes change safer.

# Go Architecture

## Workflow

1. Discover the executable entry points, modules, package graph, external boundaries, and deployment shape.
2. Ask about constraints that materially change the layout: library vs application, single binary vs multiple binaries, and monorepo vs repository-per-service.
3. Define dependency direction before naming directories.
4. Prefer explicit constructors and manual wiring in `main`; do not pass containers or service locators through business code.
5. Keep packages cohesive, break cycles at behavior boundaries, and avoid speculative interfaces.
6. Validate with `go list ./...`, `go test ./...`, and the existing build commands.

## Conditional references

- Package and system architecture: [architecture.md](references/architecture.md), [clean-architecture.md](references/clean-architecture.md), and [hexagonal-architecture.md](references/hexagonal-architecture.md)
- Domain modeling: [ddd.md](references/ddd.md)
- Manual constructor injection: [manual-di.md](references/manual-di.md)
- Directory layouts and workspaces: [directory-layouts.md](references/directory-layouts.md) and [workspaces.md](references/workspaces.md)
- Project configuration and test placement: [config.md](references/config.md) and [testing-layout.md](references/testing-layout.md)
- Resource ownership and lifecycle: [resource-management.md](references/resource-management.md)
- Streaming and large data: [data-handling.md](references/data-handling.md)
→ See `golang-refactoring` for staged structural changes and `golang-project` for CI and documentation.
