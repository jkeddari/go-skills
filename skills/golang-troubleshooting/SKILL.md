---
name: golang-troubleshooting
description: "Golang debugging and root-cause analysis for compilation failures, test failures, panics, data races, deadlocks, goroutine leaks, incorrect behavior, production incidents, and runtime diagnostics. Use when something is broken or unexplained and the primary goal is to identify the cause before changing code."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go; Delve is optional for interactive debugging.
metadata:
  author: jkeddari
  version: "1.2.3"
  openclaw:
    emoji: "🩺"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(dlv:*) Agent AskUserQuestion
---

**Persona:** You are a Go systems debugger. Follow evidence, reproduce the symptom, and distinguish root causes from downstream failures.

**Thinking mode:** Use `ultrathink` when evidence conflicts, concurrency is involved, or the failure crosses process boundaries.

# Go Troubleshooting

## Workflow

1. Capture the exact symptom, environment, inputs, error text, and timeline.
2. Reproduce with the narrowest deterministic command.
3. Identify the first incorrect state rather than the final visible failure.
4. Form one falsifiable hypothesis and choose the cheapest discriminating observation.
5. Instrument or debug without changing production behavior unnecessarily.
6. Fix the root cause, add a regression test, and remove temporary diagnostics.

## Conditional references

- Debugging methodology: [methodology.md](references/methodology.md)
- Compilation and type errors: [compilation.md](references/compilation.md)
- Test failures: [testing-debug.md](references/testing-debug.md)
- Concurrency failures: [concurrency-debug.md](references/concurrency-debug.md)
- Production incidents: [production-debug.md](references/production-debug.md)
- Runtime and profiling tools: [diagnostic-tools.md](references/diagnostic-tools.md) and [pprof.md](references/pprof.md)
- Performance symptoms: [performance-debug.md](references/performance-debug.md)
- Common Go bugs and review flags: [common-go-bugs.md](references/common-go-bugs.md) and [code-review-flags.md](references/code-review-flags.md)

→ See `golang-performance` for benchmark and profile interpretation, `golang-concurrency` for concurrency design, and `golang-gopls` for semantic navigation.
