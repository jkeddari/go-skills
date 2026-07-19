---
name: golang-database
description: "Golang relational database access with database/sql, parameterized queries, scanning, NULL handling, transactions, isolation, locking, connection pools, batch operations, migrations, context propagation, and database tests. Use when writing, reviewing, or debugging Go code that interacts with a relational database."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go and a database driver selected by the project.
metadata:
  author: jkeddari
  version: "1.2.1"
  openclaw:
    emoji: "🗄️"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent AskUserQuestion
---

**Persona:** You are a Go database reviewer. Treat transaction boundaries, query shape, resource cleanup, and concurrency semantics as part of the API contract.

# Go Database Access

## Workflow

1. Inspect schema assumptions, indexes, transaction boundaries, and the repository's chosen driver.
2. Parameterize values and allowlist any dynamic identifiers.
3. Use context-aware methods and close rows, statements, and transactions on every path.
4. Make NULL semantics explicit in domain conversion.
5. Keep transactions short and handle rollback and commit errors deliberately.
6. Review pool limits against database capacity and application concurrency.
7. Test behavior at the repository boundary without prescribing a test framework.

## Conditional references

- Scanning and type conversion: [scanning.md](references/scanning.md)
- Transactions, locking, and isolation: [transactions.md](references/transactions.md)
- Query and pool performance: [performance.md](references/performance.md)
- Database testing patterns: [testing.md](references/testing.md)

→ See `golang-security` for injection risk, `golang-concurrency` for context and shared state, and `golang-performance` for measured query bottlenecks.
