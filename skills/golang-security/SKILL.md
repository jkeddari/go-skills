---
name: golang-security
description: "Golang security engineering for threat modeling, input validation, injection prevention, authentication and authorization boundaries, cryptography, secrets, filesystem and network safety, cookies, sensitive logging, dependency exposure, and security review. Use when implementing or auditing security-sensitive Go code."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go; govulncheck is recommended for dependency exposure checks.
metadata:
  author: jkeddari
  version: "1.1.10"
  openclaw:
    emoji: "🔐"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(govulncheck:*) Agent AskUserQuestion
---

**Persona:** You are a senior Go security engineer. Trace attacker-controlled data to security-sensitive effects and report evidence, reachability, and impact.

**Thinking mode:** Use `ultrathink` for threat modeling, authorization analysis, cryptographic review, and exploitability assessment.

# Go Security

## Workflow

1. Identify assets, trust boundaries, attacker-controlled inputs, privileged effects, and deployment assumptions.
2. Trace data flows through parsing, validation, storage, commands, templates, files, network calls, and logs.
3. Confirm reachability and existing controls before reporting a vulnerability.
4. Rank findings by impact and likelihood; separate confirmed issues from hardening opportunities.
5. Recommend the smallest fix that restores a clear invariant.
6. Add a regression test or verification command where practical.

## Conditional references

Read only the category relevant to the data flow under review:

- Threat modeling: [threat-modeling.md](references/threat-modeling.md)
- Authentication, authorization, and service boundaries: [architecture.md](references/architecture.md)
- Injection: [injection.md](references/injection.md)
- Cryptography: [cryptography.md](references/cryptography.md)
- Filesystem: [filesystem.md](references/filesystem.md)
- Network and cookies: [network.md](references/network.md) and [cookies.md](references/cookies.md)
- Secrets and sensitive logging: [secrets.md](references/secrets.md) and [logging.md](references/logging.md)
- Memory safety: [memory-safety.md](references/memory-safety.md)
- Third-party and supply-chain review: [third-party.md](references/third-party.md)
- Audit checklist: [checklist.md](references/checklist.md)

→ See `golang-dependencies` for module changes, `golang-code-quality` for non-adversarial correctness, and `golang-project` for CI gates.
