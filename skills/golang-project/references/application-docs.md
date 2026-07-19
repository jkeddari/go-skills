# Application Documentation

Application documentation should let an operator discover the interface, configure the process, run it safely, and understand its external contracts without reading implementation code.

## Command help

For command-line programs, keep `-h` and `--help` complete and stable. The standard `flag` package is sufficient for simple command surfaces:

```go
package main

import (
	"flag"
	"fmt"
	"io"
)

func parseArgs(w io.Writer, args []string) error {
	fs := flag.NewFlagSet("analyze", flag.ContinueOnError)
	fs.SetOutput(w)
	input := fs.String("input", "", "path to the input file (required)")
	format := fs.String("format", "json", "output format: json or text")
	fs.Usage = func() {
		fmt.Fprintln(w, "Usage: mytool analyze -input FILE [-format json|text]")
		fmt.Fprintln(w, "Analyze FILE and write the report to standard output.")
		fs.PrintDefaults()
	}
	if err := fs.Parse(args); err != nil {
		return err
	}
	if *input == "" {
		return fmt.Errorf("-input is required")
	}
	_ = format
	return nil
}
```

Document exit codes, standard input/output behavior, destructive actions, examples, and whether flags or subcommands are stable API.

## Configuration

Document every supported source, precedence rule, default, sensitive value, reload behavior, and validation constraint. A concise table is usually enough:

| Variable | Purpose | Default | Required | Sensitive |
| --- | --- | --- | --- | --- |
| `APP_DB_URL` | Database connection string | none | yes | yes |
| `APP_LOG_LEVEL` | Log verbosity | `info` | no | no |
| `APP_HTTP_ADDR` | Listen address | `:8080` | no | no |
| `APP_TIMEOUT` | Outbound request timeout | `30s` | no | no |

Do not publish production credentials or real internal endpoints in examples. Keep configuration documentation synchronized with validation code.

## Architecture decisions

Record decisions whose rationale will otherwise be lost:

```markdown
# Use PostgreSQL as the primary store

## Context
What constraint or problem forced a decision?

## Decision
What was chosen, including the important boundary?

## Consequences
What becomes easier, harder, or operationally different?

## Alternatives
What credible options were rejected and why?
```

Avoid an architecture diary. Create a decision record only when future maintainers would reasonably revisit the choice without its context.

## HTTP and event contracts

Document contracts independently from a specific Go library:

- method or event name and semantic purpose;
- request fields, validation, defaults, and size limits;
- response schema and status or error semantics;
- authentication and authorization requirements;
- idempotency, pagination, retry, ordering, and compatibility rules;
- representative examples with fictitious data;
- deprecation and versioning policy.

Use the contract format already adopted by the project. Generated specifications should be reviewed as public API artifacts and checked into the repository only when that matches the release workflow.

## Operational runbook

For deployed applications, document:

1. startup and health checks;
2. required dependencies and failure behavior;
3. dashboards and alerts operators should consult;
4. safe restart, rollback, and recovery procedures;
5. backup and restore expectations;
6. common failure signatures and escalation ownership.

Test commands copied into documentation. A runbook that cannot be executed during an incident is worse than a short, accurate one.
