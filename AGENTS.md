# AGENTS.md

## Project

This repository is a focused Agent Skills collection for production Go engineering. It uses a hybrid architecture: one skill per meaningful engineering concern, with deeper material stored in references and loaded only when relevant.

The collection is standard-library-first. Do not add vendor-, framework-, or library-specific skills. Do not prescribe Cobra, Viper, Testify, Wire, dependency-injection containers, or similar packages. Existing project dependencies may be respected when a user explicitly places them in scope, but they must not become defaults in this collection.

## Current skills

- `golang-architecture`: boundaries, layout, configuration, and manual dependency injection
- `golang-code-quality`: style, naming, errors, interfaces, data structures, and safety
- `golang-concurrency`: goroutines, channels, synchronization, pipelines, and context
- `golang-database`: `database/sql`, transactions, migrations, performance, and tests
- `golang-dependencies`: modules, upgrades, auditing, workspaces, and conflicts
- `golang-gopls`: language-server workflows
- `golang-observability`: logs, metrics, traces, profiling exposure, and alerts
- `golang-performance`: benchmarks, profiling, optimization, and regression detection
- `golang-project`: repository bootstrap, documentation, CI, and releases
- `golang-refactoring`: behavior-preserving structural changes
- `golang-security`: secure implementation, review, and auditing
- `golang-testing`: standard-library testing workflows
- `golang-tooling`: formatting, linting, static analysis, and modernization
- `golang-troubleshooting`: evidence-driven debugging and root-cause analysis

Do not recreate removed micro-skills or optional API/framework skills. Extend the appropriate owner through a reference when the concern belongs to the catalog above.

## Repository structure

```text
skills/<skill-name>/
├── SKILL.md
├── agents/openai.yaml
├── references/       # optional Markdown, one link level from SKILL.md
├── scripts/          # optional deterministic helpers
└── assets/           # optional templates and configuration
```

Plugin manifests live in `.claude-plugin/`, `.cursor-plugin/`, and `gemini-extension.json`.

## Skill design

### Frontmatter

Every `SKILL.md` must define:

- `name`: lowercase letters, digits, and hyphens; it must match the directory name;
- `description`: 1–1024 characters, containing “Golang”, the covered concern, and concrete trigger situations;
- `license`: `MIT`;
- `compatibility`: actual runtime or tool requirements;
- `metadata.author`: `jkeddari`;
- `metadata.version`: semantic version string;
- `metadata.openclaw.emoji`, `homepage`, `requires.bins`, and `install`;
- `user-invocable`: boolean;
- `allowed-tools`: the smallest useful set.

Use `https://github.com/jkeddari/go-skills` as the skill homepage.

Descriptions are the trigger surface. Keep them specific enough to avoid loading unrelated skills and explicit enough to match real tasks. Add a boundary sentence when two neighboring skills could plausibly trigger.

### Body

Keep `SKILL.md` short and operational, preferably under 150 lines and comfortably below 2,500 tokens. It should define:

1. the mental model or posture;
2. a repeatable workflow;
3. decision boundaries and validation;
4. links to only the references needed for deeper cases.

Put command catalogs, long examples, edge cases, and reusable checklists in `references/`. Put non-Markdown templates and configuration in `assets/`. Explain when to read each linked file; do not make the agent load every reference unconditionally.

Recommendations must explain why they matter. When a hypothesis can be tested, name a diagnostic that confirms or rejects it before proposing a change.

### Ownership and cross-references

Each concept has one owner. Neighboring skills should point to the owner by local skill name, for example `golang-performance`, rather than duplicate its content. Keep performance measurement in `golang-performance`, production signals in `golang-observability`, debugging in `golang-troubleshooting`, and test design in `golang-testing`.

Cross-references must not depend on a previous repository owner or URL.

### Dependencies

Prefer Go's standard library and official Go tools. A skill may document an external tool only when it is intrinsic to the concern, optional, and declared in compatibility and allowed tools. Do not create library catalogs or recommend dependencies merely for convenience.

Tests must use `testing` and handwritten fakes by default. Do not introduce Testify or mock frameworks. Architecture guidance uses explicit constructors and composition roots rather than dependency-injection libraries.

## Validation

Before considering a change complete:

1. Confirm that every skill directory contains a valid `SKILL.md`.
2. Confirm that frontmatter names match directory names and all required fields exist.
3. Confirm that descriptions contain “Golang” and stay below 1,024 characters.
4. Resolve every relative Markdown link from the file that declares it.
5. Search for deleted skill names, stale owner/repository references, and banned library defaults.
6. Run Markdown linting when available.
7. Review `git diff` to ensure moved references were not duplicated or orphaned.

New skills begin at `1.0.0`. Modifying a published skill or plugin requires a deliberate semantic-version increment before release; do not bump versions incidentally during content edits.
