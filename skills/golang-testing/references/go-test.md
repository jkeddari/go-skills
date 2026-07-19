# `go test` Reference

Start narrow for fast feedback, then widen once the focused behavior passes.

## Selection and repetition

```bash
go test ./...                                  # all packages in the module
go test ./path/to/package                     # one package
go test -run '^TestParse$' ./path/to/package  # exact test
go test -run 'TestParse/empty' ./...           # named subtest
go test -count=1 ./...                         # bypass cached success
go test -count=100 -run TestFlaky ./...        # repeat a suspected flaky test
go test -failfast ./...                        # stop package after first failure
go test -timeout 2m ./...                      # bound the package test binary
```

`-run` is a regular expression matched against slash-separated test and subtest names. Anchor it when an exact selection matters.

## Diagnostics

```bash
go test -v ./...
go test -json ./... > test-events.json
go test -race ./...
go test -shuffle=on ./...
go test -run TestName -trace trace.out ./path/to/package
go tool trace trace.out
```

Use `-shuffle=on` to expose order coupling and `-race` for code that shares memory across goroutines. Neither proves the absence of ordering bugs or races that the tests never exercise.

## Coverage

```bash
go test -cover ./...
go test -coverprofile cover.out ./...
go tool cover -func cover.out
go tool cover -html cover.out
go test -covermode=atomic -coverprofile cover.out ./...  # concurrent coverage
```

Coverage identifies code not executed by the suite; it does not show whether assertions constrain the correct behavior. Review uncovered error and boundary paths instead of optimizing only for a percentage.

## Fuzzing

```bash
go test -run '^$' -fuzz '^FuzzDecode$' ./path/to/package
go test -run '^$' -fuzz '^FuzzDecode$' -fuzztime 30s ./path/to/package
```

Seed the corpus with valid, invalid, empty, boundary, and historically failing values. Assert stable properties such as no panic, bounded output, round-trip behavior, or rejection of invalid structure. Keep minimized regressions under `testdata/fuzz/`.

## Integration tests

```bash
go test -tags=integration ./...
go test -tags=integration -run TestRepository ./internal/store
```

Use a build tag when the suite requires infrastructure unavailable in the normal unit-test loop. Make prerequisites, isolation, cleanup, and timeouts explicit; a skipped integration suite must be visible in CI output.

## Useful test-binary flags

```bash
go test -args -test.list . ./path/to/package
go test -cpu=1,2,4 ./...
go test -parallel=4 ./...
go test -short ./...
```

Package flags appear before `-args`; flags after it are passed to the test binary. Prefer ordinary `go test` flags unless the package deliberately defines custom test inputs.

## Cache awareness

A successful package result may be cached when the command and relevant inputs are unchanged. Use `-count=1` when reproducing nondeterminism or when an external integration dependency changed without a source change. Do not disable caching globally; fast deterministic tests should benefit from it.
