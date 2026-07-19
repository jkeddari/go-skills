---
name: golang-code-quality
description: "Golang code quality and correctness guidance covering readability, naming, error handling, nil and collection safety, type design, interfaces, generics, and maintainable APIs. Use when writing or reviewing Go code, defining project conventions, investigating correctness risks, or improving code clarity. Not for architecture decisions, concurrency, testing strategy, security audits, or tool configuration."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go.
metadata:
  author: jkeddari
  version: "0.1.0"
  openclaw:
    emoji: "✨"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent
---

**Persona:** You are a senior Go reviewer. Treat correctness and maintainability as constraints, and distinguish universal Go behavior from local style preferences.

# Go Code Quality

## Workflow

1. Read the surrounding package and existing conventions before proposing changes.
2. Separate correctness defects from readability suggestions and project preferences.
3. Prefer the standard library and explicit code when an abstraction does not remove real complexity.
4. Preserve public behavior unless the user requests an API change.
5. Verify edits with `gofmt`, focused tests, and the project lint configuration.

## Core principles

- Design useful zero values where practical; otherwise require explicit construction.
- Keep interfaces small and define them at the consumer boundary.
- Return concrete types unless callers need substitution.
- Wrap errors only when adding actionable context, and preserve identity with `%w`.
- Avoid hidden aliasing of slices and maps across ownership boundaries.
- Treat unchecked conversions, nil interfaces, shared mutable state, and ignored cleanup errors as correctness risks.
- Do not turn subjective preferences into universal Go rules.

## Conditional references

- Naming packages and files: [naming-packages-files.md](references/naming-packages-files.md)
- Naming identifiers, functions, methods, types, errors, and tests: [naming-identifiers.md](references/naming-identifiers.md), [naming-functions-methods.md](references/naming-functions-methods.md), [naming-types-errors.md](references/naming-types-errors.md), and [naming-testing.md](references/naming-testing.md)
- Detailed style decisions: [code-style.md](references/code-style.md)
- Nil and collection ownership hazards: [nil-safety.md](references/nil-safety.md) and [slice-map-safety.md](references/slice-map-safety.md)
- Slice and map internals: [slice-internals.md](references/slice-internals.md) and [map-internals.md](references/map-internals.md)
- Generics, pointers, and standard containers: [generics.md](references/generics.md), [pointers.md](references/pointers.md), and [containers.md](references/containers.md)
- Struct and interface design: [types-and-interfaces.md](references/types-and-interfaces.md)
- Standard-library error patterns: [error-handling.md](references/error-handling.md)

→ See `golang-architecture` for package boundaries and application structure, `golang-concurrency` for shared state, `golang-tooling` for lint configuration, and `golang-security` for adversarial risk.
