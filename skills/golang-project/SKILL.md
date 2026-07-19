---
name: golang-project
description: "Golang project delivery guidance covering GitHub Actions CI, tests, linting, security checks, coverage, releases, repository settings, README files, contribution guides, changelogs, doc comments, and operational documentation. Use when setting up or reviewing project automation and documentation."
user-invocable: true
license: MIT
compatibility: Designed for Codex or similar AI coding agents. Requires Go and git; CI assets target GitHub Actions.
metadata:
  author: jkeddari
  version: "1.0.0"
  openclaw:
    emoji: "🚀"
    homepage: "https://github.com/jkeddari/go-skills"
    requires:
      bins: [go, git]
    install: []
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Bash(goreleaser:*) Bash(gh:*) Agent AskUserQuestion
---

# Go Project Delivery

## Workflow

1. Detect whether the repository is a library, service, CLI, or monorepo and identify its supported Go versions.
2. Preserve existing CI conventions unless the user requests a migration.
3. Keep CI jobs deterministic, least-privileged, pinned to reviewed action versions, and scoped to useful quality gates.
4. Document how to build, test, configure, run, release, and recover the project.
5. Keep comments focused on contracts and non-obvious reasoning; let code express mechanics.
6. Validate workflow syntax and run the documented local commands before handing off.

## Conditional references

- Repository security settings: [repo-security.md](references/repo-security.md)
- Project-level documentation: [project-docs.md](references/project-docs.md)
- Application documentation: [application-docs.md](references/application-docs.md)
- Library documentation: [library-docs.md](references/library-docs.md)
- Go comments and examples: [code-comments.md](references/code-comments.md)
- CI workflow assets: use only the relevant files under [assets/ci](assets/ci)
- Documentation templates: use only the relevant files under [assets/docs](assets/docs)

→ See `golang-tooling` for lint configuration, `golang-testing` for test strategy, `golang-dependencies` for update policy, and `golang-security` for security gates.
