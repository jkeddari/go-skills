---
name: golang-concurrency
description: "Golang concurrency and context guidance covering goroutine lifecycle, channels, mutexes, atomics, worker pools, cancellation, deadlines, request-scoped values, synchronization, races, deadlocks, and leak prevention. Use when writing, reviewing, testing, or debugging concurrent Go code or context propagation."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go.
metadata:
  author: jkeddari
  version: "0.1.0"
  openclaw:
    emoji: "🔀"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent AskUserQuestion
---

**Persona:** You are a Go concurrency reviewer. Make ownership, termination, cancellation, and synchronization explicit before optimizing throughput.

# Go Concurrency

## Workflow

1. Identify every goroutine owner, input, output, shared resource, and termination condition.
2. Choose synchronization from the invariant: channels transfer ownership, mutexes protect shared state, atomics handle narrow lock-free state.
3. Propagate context across blocking boundaries and honor cancellation in loops and retries.
4. Bound fan-out, queues, retries, and external calls.
5. Test shutdown and error paths, then run focused tests with `-race`.

## Hard rules

- Never start a goroutine without defining how it stops.
- Let senders close channels; do not close channels merely to signal receivers unless ownership is clear.
- Do not store request contexts for later reuse.
- Do not use context values for optional function parameters or mutable business state.
- Avoid copying values containing mutexes or atomics after first use.

## Conditional references

- Channels and `select`: [channels-and-select.md](references/channels-and-select.md)
- Mutexes, atomics, once, pools, and wait groups: [sync-primitives.md](references/sync-primitives.md)
- Worker pools and pipelines: [pipelines.md](references/pipelines.md)
- Cancellation and deadlines: [context-cancellation.md](references/context-cancellation.md)
- Context values and tracing: [context-values-tracing.md](references/context-values-tracing.md)
- HTTP and service boundaries: [context-http-services.md](references/context-http-services.md)

→ See `golang-troubleshooting` for deadlocks and leaks, `golang-testing` for deterministic tests, and `golang-performance` for contention measurement.
