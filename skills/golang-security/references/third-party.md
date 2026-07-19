# External Data and Supply-Chain Review

Third-party services and modules expand both the data boundary and the software supply chain. Review the boundary itself instead of assuming a popular provider or package is safe.

## Data leaving the process

For telemetry, error reporting, analytics, search, storage, and external APIs, determine:

1. which fields leave the process;
2. whether they contain credentials, tokens, session identifiers, personal data, payload bodies, or internal topology;
3. the destination region, retention period, access model, and deletion path;
4. behavior when redaction or transmission fails;
5. whether sampling can accidentally retain the most sensitive events.

Apply redaction before data reaches a provider-specific client. A small local boundary makes the policy testable:

```go
type EventSink interface {
	Record(context.Context, Event) error
}

type Event struct {
	Name       string
	SubjectID  string
	Attributes map[string]string
}

func PublicAttributes(src map[string]string) map[string]string {
	dst := make(map[string]string, len(src))
	for key, value := range src {
		switch strings.ToLower(key) {
		case "authorization", "cookie", "password", "secret", "token":
			dst[key] = "[REDACTED]"
		default:
			dst[key] = value
		}
	}
	return dst
}
```

An allowlist is safer than a denylist when the event schema is small, because newly added sensitive fields otherwise pass through by default.

## Module review

Before adding a production module, evaluate:

- whether the standard library already satisfies the requirement;
- the minimum API surface actually needed;
- transitive module and toolchain impact;
- privileged operations such as filesystem, network, process, or credential access;
- maintenance and replacement cost;
- license and distribution constraints;
- how the dependency will be pinned, audited, upgraded, and removed.

Treat vulnerability scanner output as a lead. Confirm that the affected package and symbol are reachable in the deployed build before assigning impact, while still respecting cases where configuration or data files make a flaw exploitable without a direct call.

## Verification

```bash
go mod verify
go list -m all
go mod graph
govulncheck ./...
```

Review `go.mod` and `go.sum` in the same change as the code that introduces the dependency. Unexpected transitive modules or toolchain directives are part of the security review, not incidental noise.
