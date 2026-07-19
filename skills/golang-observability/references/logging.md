# Structured logging with `log/slog`

## Establish a stable schema

Use short event messages and stable attribute names. Prefer values that can be filtered and aggregated.

```go
logger := slog.New(slog.NewJSONHandler(os.Stdout, &slog.HandlerOptions{
    Level: slog.LevelInfo,
}))

logger.ErrorContext(ctx, "payment failed",
    "order_id", orderID,
    "provider", provider,
    "error", err,
)
```

## Levels

- `Debug`: temporary diagnostic detail, normally disabled in production.
- `Info`: successful lifecycle and domain events useful to operators.
- `Warn`: degraded behavior or a condition approaching failure.
- `Error`: the current operation failed.

Do not encode severity in the message text. Configure level filtering at the handler.

## Boundaries

Log an error at the boundary that owns the operation. Lower layers should return contextual errors rather than logging and returning the same failure repeatedly.

Use `ErrorContext`, `InfoContext`, and other context-aware methods when a handler enriches records from context. Keep context enrichment small and controlled.

## Sensitive data

Never log credentials, authorization headers, session tokens, private keys, full payment data, or raw personal records. Prefer stable internal identifiers and redact values before they reach the logger.

## Volume and cost

Avoid per-item logs in hot loops. Prefer counters and histograms for high-volume behavior. If sampling is required, implement it at the logging boundary and document which events may be dropped.

## Testing

Use a small custom `slog.Handler` in tests to capture records and assert stable keys. Avoid assertions on timestamps or attribute ordering.
