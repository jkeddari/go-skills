---
name: golang-observability
description: "Golang production observability using structured slog logging, metrics, distributed traces, runtime profiling, dashboards, alerts, and correlation across signals. Use when instrumenting a Go service, defining service-level indicators, diagnosing missing production visibility, or reviewing logging, metrics, tracing, and alerting design."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go.
metadata:
  author: jkeddari
  version: "0.1.0"
  openclaw:
    emoji: "📡"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent AskUserQuestion
---

**Persona:** You are a Go reliability engineer. Instrument decisions and user-visible behavior, not every line of code.

# Go Observability

## Workflow

1. Identify the service boundary, critical user journeys, failure modes, and operational owners.
2. Define useful signals before adding instrumentation.
3. Use structured `log/slog` records with stable keys and appropriate levels.
4. Measure request rate, errors, latency, saturation, and domain outcomes where they drive action.
5. Propagate trace context across process boundaries and attach errors without leaking secrets.
6. Add alerts only when an operator can take a defined action.
7. Verify cardinality, sampling, privacy, cost, and behavior during partial failure.

## Conditional references

- Structured logging: [logging.md](references/logging.md)
- Metrics and cardinality: [metrics.md](references/metrics.md)
- Distributed tracing: [tracing.md](references/tracing.md)
- Alert design: [alerting.md](references/alerting.md)
- Dashboards: [dashboards.md](references/dashboards.md)
- Runtime and continuous profiling: [profiling.md](references/profiling.md)
→ See `golang-performance` for temporary profiling investigations, `golang-security` for sensitive data, and `golang-troubleshooting` for incident diagnosis.
