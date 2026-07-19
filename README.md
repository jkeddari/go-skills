# Go Engineering Skills

A focused collection of Agent Skills for production Go work. The repository uses a hybrid architecture: broad concerns are grouped into a small number of coherent skills, while detailed guidance lives in references loaded only when needed.

The collection is intentionally standard-library-first. It does not prescribe application frameworks, CLI libraries, assertion libraries, or vendor-specific packages.

## Install

```bash
npx skills add https://github.com/jkeddari/go-skills --all
```

Install a single skill when only one concern is useful:

```bash
npx skills add https://github.com/jkeddari/go-skills --skill golang-testing
```

## Architecture

The 14 skills are large enough to own a real engineering concern and small enough to trigger independently. Their `SKILL.md` files contain the operating workflow; detailed commands, examples, and edge cases are kept in `references/`.

| Skill | Purpose |
| --- | --- |
| `golang-architecture` | Package boundaries, project layout, dependency direction, configuration, and manual dependency injection |
| `golang-code-quality` | Idiomatic style, naming, errors, interfaces, data structures, and defensive coding |
| `golang-concurrency` | Goroutines, channels, synchronization, pipelines, cancellation, and context propagation |
| `golang-database` | `database/sql`, transactions, query safety, migrations, performance, and database tests |
| `golang-dependencies` | Go modules, upgrades, vulnerability review, workspaces, and dependency conflicts |
| `golang-gopls` | Language-server setup, navigation, diagnostics, refactoring, and editor integration |
| `golang-observability` | Structured logging, metrics, tracing, profiling exposure, and actionable alerts |
| `golang-performance` | Benchmark design, profiling, compiler diagnostics, optimization, and regression detection |
| `golang-project` | Repository bootstrap, documentation, CI, releases, and maintenance files |
| `golang-refactoring` | Behavior-preserving structural changes with explicit safety nets and validation |
| `golang-security` | Threat-focused implementation, review, auditing, and secure defaults |
| `golang-testing` | Standard-library unit, table-driven, integration, fuzz, race, and coverage workflows |
| `golang-tooling` | Formatting, static analysis, linting, and Go-version modernization |
| `golang-troubleshooting` | Reproduction, evidence collection, Delve, runtime diagnostics, pprof, and root-cause analysis |

The split is deliberate:

- concerns with distinct triggers remain separate, such as testing, security, and performance;
- closely coupled micro-skills are consolidated, such as naming, errors, and style under code quality;
- detailed material is disclosed progressively instead of loading one monolithic Go manual;
- framework- and vendor-specific skills are excluded.

## Layout

```text
skills/<skill-name>/
├── SKILL.md          # trigger metadata and focused workflow
├── agents/openai.yaml # optional UI metadata
├── references/       # deeper guidance loaded on demand
└── assets/           # reusable configs and templates
```

## Contributing

Keep every concept owned by one skill. Cross-reference the owning skill instead of duplicating instructions. Descriptions must state both what the skill covers and when it should trigger.

Before publishing changes, validate frontmatter, relative links, Markdown, and stale cross-references. Skill and plugin versions should be incremented deliberately as part of the release process.

## License

MIT. See [LICENSE](LICENSE).
