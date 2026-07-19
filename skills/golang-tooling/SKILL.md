---
name: golang-tooling
description: "Golang linting and modernization workflows using gofmt, go vet, static analysis, golangci-lint, compiler diagnostics, and recent standard-library features. Use when configuring linters, interpreting diagnostics, handling nolint directives, upgrading a Go version, removing deprecated patterns, or modernizing existing Go code."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go and golangci-lint.
metadata:
  author: jkeddari
  version: "1.0.0"
  openclaw:
    emoji: "🛠️"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go, golangci-lint]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent
---

# Go Tooling

## Workflow

1. Read the module Go version and existing lint configuration.
2. Reproduce the diagnostic with the narrowest relevant command.
3. Understand whether the finding represents correctness, compatibility, performance, or style.
4. Change source code deliberately; do not apply broad automatic fixes without reviewing the diff.
5. Re-run the focused check, format modified files, then run affected tests.

## Rules

- Treat the repository configuration as the source of truth.
- Require a reason for every `nolint` directive and keep its scope minimal.
- Do not modernize code beyond the module's supported Go version.
- Separate mechanical modernization from behavioral refactoring.
- Prefer standard-library replacements only when semantics remain equivalent.

## Conditional references

- Linter selection and commands: [linter-reference.md](references/linter-reference.md)
- Suppression policy: [nolint-directives.md](references/nolint-directives.md)
- Version-specific language and library changes: [modernize-versions.md](references/modernize-versions.md)
- Modernization tooling: [modernize-tooling.md](references/modernize-tooling.md)
- Reusable lint configuration: [.golangci.yml](assets/.golangci.yml)

→ See `golang-gopls` for semantic navigation and safe rename, and `golang-refactoring` for structural changes.
