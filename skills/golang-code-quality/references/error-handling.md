# Standard-library error handling

## Preserve meaning

- Return an error when the caller can decide how to handle failure.
- Add context at meaningful boundaries and preserve identity with `%w`.
- Use `errors.Is` for sentinel identity and `errors.As` for typed behavior.
- Use `errors.Join` only when callers benefit from observing multiple independent failures.

```go
if err != nil {
    return fmt.Errorf("load account %q: %w", id, err)
}
```

## Define contracts deliberately

- Use sentinel errors for stable package-level conditions callers must branch on.
- Use custom error types when callers need structured fields or behavior.
- Keep error strings lowercase, concise, and free of trailing punctuation when they are intended for wrapping.
- Do not expose internal paths, queries, credentials, or personal data in user-facing errors.

## Handle once

Return errors through reusable layers and log them at the boundary that owns the operation. Logging and returning the same failure at every layer creates duplicate, misleading events.

## Cleanup and partial failure

Do not discard meaningful `Close`, `Flush`, `Commit`, or rollback errors. When a primary failure and cleanup failure both matter, preserve both without hiding the original cause.

## Panic and recover

Reserve panic for broken invariants or unrecoverable initialization failures. Recover only at a boundary that can restore a valid state, and convert the panic into a controlled failure with sufficient internal diagnostics.
