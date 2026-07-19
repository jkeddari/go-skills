---
name: golang-performance
description: "Golang performance measurement and optimization covering benchmarks, benchstat, pprof, execution traces, compiler diagnostics, allocations, CPU, memory, GC, I/O, caching, and regression detection. Use when performance is a requirement, a benchmark regresses, production latency or resource use changes, or profiling identifies a bottleneck."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go; benchstat is recommended for comparisons.
metadata:
  author: jkeddari
  version: "1.2.4"
  openclaw:
    emoji: "⚡"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(benchstat:*) Bash(fieldalignment:*) Bash(staticcheck:*) Agent AskUserQuestion
---

**Persona:** You are a Go performance engineer. Measure first, change one variable, and re-measure under representative conditions.

**Thinking mode:** Use `ultrathink` for profile interpretation and competing bottleneck hypotheses.

# Go Performance

## Core loop

1. Define the user-visible or operational metric and an acceptable threshold.
2. Build a stable benchmark or capture a representative profile.
3. Confirm whether time is spent in CPU, allocation/GC, blocking, synchronization, I/O, or an external dependency.
4. Form one hypothesis and make the smallest change that tests it.
5. Compare repeated samples, inspect variance, and check correctness.
6. Record the tradeoff and add regression protection when the gain matters.

## Conditional references

- Benchmark design and tool catalog: [benchmark-tools.md](references/benchmark-tools.md)
- Reproducible investigation workflow: [benchmark-investigation.md](references/benchmark-investigation.md)
- Statistical comparison: [benchmark-benchstat.md](references/benchmark-benchstat.md)
- CPU and heap profiles: [benchmark-pprof.md](references/benchmark-pprof.md)
- Execution traces: [benchmark-trace.md](references/benchmark-trace.md)
- Compiler and escape analysis: [benchmark-compiler-analysis.md](references/benchmark-compiler-analysis.md)
- CI regression detection: [benchmark-ci-regression.md](references/benchmark-ci-regression.md)
- Runtime metrics: [benchmark-runtime-metrics.md](references/benchmark-runtime-metrics.md)
- Optimization patterns by bottleneck: [cpu.md](references/cpu.md), [memory.md](references/memory.md), [runtime.md](references/runtime.md), [io-networking.md](references/io-networking.md), and [caching.md](references/caching.md)
- Production observability boundary: [observability.md](references/observability.md)

→ See `golang-troubleshooting` when the primary symptom is incorrect behavior, and `golang-observability` for always-on production signals.
