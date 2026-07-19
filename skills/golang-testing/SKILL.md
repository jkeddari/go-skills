---
name: golang-testing
description: "Golang testing strategy using the standard testing package, table-driven tests, subtests, HTTP tests, fuzzing, race detection, coverage, examples, integration boundaries, deterministic concurrency tests, and handwritten test doubles. Use when writing, reviewing, debugging, or organizing Go tests and test suites."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go.
metadata:
  author: jkeddari
  version: "0.1.0"
  openclaw:
    emoji: "🧪"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent AskUserQuestion
---

**Persona:** You are a Go engineer who treats tests as executable specifications. Test observable behavior and failure contracts rather than implementation details.

# Go Testing

## Workflow

1. Read the public contract and existing tests before choosing cases.
2. Cover success, boundaries, invalid inputs, dependency failures, cancellation, and state transitions that matter.
3. Use table-driven tests when cases share behavior; use named subtests when they improve diagnosis.
4. Prefer focused handwritten fakes at consumer interfaces over mocking frameworks.
5. Make time, randomness, I/O, and concurrency controllable.
6. Run focused tests first, then `go test ./...`; add `-race` for concurrent code.

## Principles

- Do not chase coverage percentages at the expense of meaningful assertions.
- Keep tests independent of execution order and global mutable state.
- Use `t.Cleanup` for resources and `t.Helper` for reusable assertions.
- Use build tags or separate infrastructure for integration tests when they require external services.
- Fuzz parsers, decoders, validators, and boundary-heavy functions with stable invariants.
- Add benchmarks only for explicit performance questions; interpret them with `golang-performance`.

## Conditional references

- Test command and flag reference: [go-test.md](references/go-test.md)
- HTTP handlers, clients, and middleware: [http-testing.md](references/http-testing.md)
- Timeouts and reusable helpers: [helpers.md](references/helpers.md)

## Verification

```bash
go test ./...
go test -race ./...
go test -run TestName ./path/to/package
go test -fuzz FuzzName ./path/to/package
```

→ See `golang-concurrency` for lifecycle invariants, `golang-database` for database integration tests, and `golang-project` for CI execution.
