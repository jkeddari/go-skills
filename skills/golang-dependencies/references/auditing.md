# Dependency Auditing

Audit the dependency graph as code: determine why each production module exists, which symbols are reachable, what privilege it receives, and how it will be upgraded or removed.

## Reproducibility and graph inspection

```bash
go mod verify
go mod tidy -diff
go list -m all
go mod graph
go mod why -m example.com/module
go list -deps -json ./...
```

Review unexpected changes to `go.mod`, `go.sum`, the `go` directive, the `toolchain` directive, replace directives, and tool dependencies. `go mod tidy` making a change is a prompt to understand the graph, not a reason to accept it automatically.

## Vulnerability review

```bash
govulncheck ./...
```

For every finding, record the affected module and symbol, whether the deployed build reaches it, the conditions needed to exploit it, and the smallest safe remediation. Reachability reduces false urgency but does not prove safety when a flaw is triggered through configuration, generated data, plugins, or a process boundary.

## New dependency checklist

- Can the standard library or existing module satisfy the requirement?
- Is the imported surface narrow and located behind a local boundary?
- What transitive modules and build tags are introduced?
- Does initialization perform network, filesystem, process, or credential access?
- Does it add cgo, code generation, or platform constraints?
- Are license and distribution obligations compatible with the project?
- Can the dependency be replaced without changing domain APIs?

## Binary-size diagnosis

Build comparable binaries before attributing size to a module:

```bash
go build -o app ./cmd/app
go tool nm -size app | sort -k2,2nr | head
go build -trimpath -ldflags='-s -w' -o app-stripped ./cmd/app
```

Debug tables and embedded assets can dominate size. Map large symbols back to packages before removing a dependency.
