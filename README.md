# Go Engineering Skills

A focused collection of Agent Skills for production Go work. The repository uses a hybrid architecture: broad concerns are grouped into a small number of coherent skills, while detailed guidance lives in references loaded only when needed.

The collection is intentionally standard-library-first. It does not prescribe application frameworks, CLI libraries, assertion libraries, or vendor-specific packages.

## Manual installation

Clone the repository once, then copy the desired skill directories from `skills/` into the discovery directory used by your agent. Copy the whole skill directory: `SKILL.md`, `references/`, `assets/`, and `agents/` belong together.

```bash
git clone --depth 1 git@github.com:jkeddari/go-skills.git /tmp/go-skills
```

### Codex

Install globally for all projects:

```bash
mkdir -p ~/.codex/skills
cp -R /tmp/go-skills/skills/. ~/.codex/skills/
```

Or install only for the current repository:

```bash
mkdir -p .codex/skills
cp -R /tmp/go-skills/skills/. .codex/skills/
```

### Claude Code

```bash
# Global
mkdir -p ~/.claude/skills
cp -R /tmp/go-skills/skills/. ~/.claude/skills/

# Or project-local
mkdir -p .claude/skills
cp -R /tmp/go-skills/skills/. .claude/skills/
```

### Cursor

```bash
# Global
mkdir -p ~/.cursor/skills
cp -R /tmp/go-skills/skills/. ~/.cursor/skills/

# Or project-local
mkdir -p .cursor/skills
cp -R /tmp/go-skills/skills/. .cursor/skills/
```

### Gemini CLI

```bash
# Global
mkdir -p ~/.gemini/skills
cp -R /tmp/go-skills/skills/. ~/.gemini/skills/

# Or project-local
mkdir -p .gemini/skills
cp -R /tmp/go-skills/skills/. .gemini/skills/
```

### Agent Skills-compatible agents

Agents that discover the shared Agent Skills convention can use a project-local `.agents/skills/` directory:

```bash
mkdir -p .agents/skills
cp -R /tmp/go-skills/skills/. .agents/skills/
```

To install only one skill, replace `skills/.` in a command above with its directory, for example `skills/golang-testing`.

## Plugin installation

This repository also includes plugin manifests for Claude Code, Cursor, Gemini CLI, and Codex. A plugin installation keeps the skill collection together and lets the agent discover the complete `skills/` directory.

### Claude Code

In Claude Code, install the repository as a plugin:

```text
/plugin install https://github.com/jkeddari/go-skills
```

Restart Claude Code or begin a new conversation after installation.

### Cursor

Open **Settings → Plugins**, choose the option to install a plugin from GitHub, then enter:

```text
https://github.com/jkeddari/go-skills
```

Restart Cursor or open a new agent conversation after the plugin is installed. If the plugin source option is unavailable in your Cursor build, use the manual `.cursor/skills/` installation above.

### Gemini CLI

Install the repository extension:

```bash
gemini extensions install https://github.com/jkeddari/go-skills
```

Start a new Gemini CLI session after installation.

### Codex

Codex plugins are installed from a marketplace. Clone the repository in the location expected by your personal marketplace:

```bash
mkdir -p ~/plugins
git clone https://github.com/jkeddari/go-skills.git ~/plugins/go-skills
mkdir -p ~/.agents/plugins
```

If `~/.agents/plugins/marketplace.json` does not exist, create it with:

```json
{
  "name": "personal",
  "interface": {
    "displayName": "Personal"
  },
  "plugins": [
    {
      "name": "go-skills",
      "source": {
        "source": "local",
        "path": "./plugins/go-skills"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Developer Tools"
    }
  ]
}
```

If the file already exists, append the `go-skills` object from the example to its `plugins` array instead of replacing existing entries. Then install the plugin:

```bash
codex plugin add go-skills@personal
```

Start a new Codex conversation after installation so that the skills are discovered. Use the manual `~/.codex/skills/` route above when you only need the skills and not the plugin packaging.

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
