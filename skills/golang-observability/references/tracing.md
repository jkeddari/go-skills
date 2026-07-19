# Distributed tracing

## Span boundaries

Create spans around meaningful operations: inbound requests, outbound calls, queue processing, database work, and expensive domain operations. Avoid spans for trivial helpers.

```go
ctx, span := tracer.Start(ctx, "OrderService.Create")
defer span.End()

order, err := repo.Insert(ctx, input)
if err != nil {
    span.RecordError(err)
    span.SetStatus(codes.Error, "insert failed")
    return Order{}, fmt.Errorf("insert order: %w", err)
}
```

## Propagation

Propagate the returned context into every downstream call. Extract remote context at ingress and inject it at egress using the transport's standard propagation mechanism.

## Attributes

Use low-cardinality attributes that support investigation: operation type, dependency, result class, region, or stable internal IDs. Do not attach secrets, request bodies, unbounded query text, or personal data.

## Errors

Record the error on the span and preserve Go error identity in the returned error. Do not mark expected business outcomes as system errors unless operators should act on them.

## Sampling

Choose sampling from traffic volume, investigation needs, and cost. Parent-based sampling usually preserves complete distributed traces. Document which high-value failures must remain observable.

## Verification

Test propagation across an in-memory client/server boundary and verify that child spans share the expected trace ID. Check exporter failure behavior so telemetry cannot block the application indefinitely.
