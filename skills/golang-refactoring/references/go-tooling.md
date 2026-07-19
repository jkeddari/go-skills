# Go Tooling for Refactoring

Choose the least powerful semantic tool that can perform the transformation safely. Text replacement is suitable only when the target is intentionally textual; it cannot distinguish code from comments, strings, generated files, or unrelated identifiers.

## gopls

Use `gopls` for type-aware rename, references, implementations, call hierarchy, extract, inline, and diagnostics. Inspect the proposed edit and affected packages before accepting a workspace-wide action.

```bash
gopls references ./path/file.go:#byte-offset
gopls rename ./path/file.go:#byte-offset NewName
gopls check ./path/...
```

The exact CLI surface varies by version. Prefer an editor or LSP client when it can preview workspace edits. See `golang-gopls` for the semantic workflow.

## gofmt rewrites

`gofmt -r` is useful for a repeated single-expression rewrite:

```bash
gofmt -r 'bytes.Compare(a, b) == 0 -> bytes.Equal(a, b)' -d file.go
gofmt -r 'bytes.Compare(a, b) == 0 -> bytes.Equal(a, b)' -w file.go
gofmt -s -w file.go
```

It is syntax-aware but not type-aware. Preview the diff first and restrict the file set when another package can expose a look-alike expression.

## Official analysis packages

For a repository-specific, type-aware bulk change, build a small analyzer with `golang.org/x/tools/go/analysis` and attach `SuggestedFix` edits to diagnostics. Test it against golden files before running it on production code. This costs more than a manual change, so reserve it for repeated transformations where reviewability and reproducibility justify the helper.

The standard AST packages are sufficient for many controlled rewrites:

- `go/parser` parses source;
- `go/ast` represents and walks syntax;
- `go/token` preserves source positions;
- `go/types` adds semantic type information;
- `go/format` produces canonical output.

Comments are position-sensitive. After an AST rewrite, inspect comment attachment and formatting instead of assuming a successful parse preserved intent.

## Blast-radius discovery

```bash
go list -deps ./...
go list -json ./path/to/package
go mod graph
go test ./path/to/affected/...
```

Combine these with gopls definitions, references, implementations, and call hierarchy. Search separately for non-semantic coupling such as configuration keys, serialized field names, templates, generated code, reflection, and shell scripts.

## Verification after a mechanical change

```bash
gofmt -w ./affected/files
go vet ./affected/...
go test ./affected/...
go test ./...
git diff --check
git diff
```

Compilation proves type consistency, not behavioral equivalence. Keep the behavioral safety net from [safety-net.md](safety-net.md) active throughout the refactor.
