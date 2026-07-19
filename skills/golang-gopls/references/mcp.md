# gopls Integration Options

`gopls` can be exposed through an editor's LSP client, an agent-specific semantic tool, or its command-line interface. Capabilities are similar, but names and transport details vary by client.

## Setup

```bash
go install golang.org/x/tools/gopls@latest
gopls version
gopls check ./...
```

Start the language server from the workspace root with the environment and build tags used by the project. A client normally launches `gopls serve` over standard input and output.

## Capability mapping

| Goal | LSP capability | CLI or fallback |
| --- | --- | --- |
| definition | `textDocument/definition` | `gopls definition` |
| references | `textDocument/references` | `gopls references` |
| implementations | `textDocument/implementation` | semantic client operation |
| rename | `textDocument/rename` | `gopls rename` |
| symbols | document or workspace symbol | `gopls symbols` where supported |
| call graph | call hierarchy | semantic client operation |
| diagnostics | published diagnostics | `gopls check` |
| code actions | `textDocument/codeAction` | client-specific execution |

Agent integrations may expose friendly names such as search, package API, file context, references, diagnostics, and rename. Map them by semantic purpose instead of assuming one tool server's exact command names.

## Safe workflow

1. Confirm the workspace and build configuration loaded successfully.
2. Locate the definition and inspect diagnostics before editing.
3. Find references and implementations before changing a public symbol or interface.
4. Preview workspace edits when the client supports it.
5. Re-run diagnostics and affected tests after the change.

## Failure modes

- A missing definition can mean the file is excluded by build tags or the wrong module root is open.
- Stale diagnostics can require the client to notify `gopls` of changed files.
- Generated files may be intentionally excluded from rename or formatting.
- Reflection, string keys, templates, and configuration are not semantic references; search them separately.
- Different `GOOS`, `GOARCH`, cgo, or tags can expose a different program than the editor sees.

When semantic integration is unavailable, fall back to `gopls` CLI operations, then to `go list`, compiler diagnostics, and carefully scoped text search.
