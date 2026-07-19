# Metrics and Cardinality

Metrics should answer an operational question over time. Define the question, unit, aggregation, and owner before choosing an exporter or backend.

## Signal model

For a service boundary, begin with:

- request or job rate;
- error rate classified by actionable cause;
- latency distribution, including tail latency;
- saturation of bounded resources such as workers, queues, connections, memory, and CPU;
- domain outcomes that indicate whether users succeeded.

Counters represent cumulative events. Gauges represent current state. Histograms represent distributions. Avoid averaging client-side when a distribution is required; averages hide multimodal and tail behavior.

## Stable names and units

- Use base units such as seconds and bytes.
- Describe what was counted, not the implementation that counted it.
- Keep label keys stable across releases.
- Do not encode dynamic values into metric names.
- Publish a metric only when its interpretation and owner are known.

## Cardinality budget

Every unique label combination creates a time series. Never attach unbounded values such as user IDs, request IDs, raw URLs, SQL text, error messages, IP addresses, or timestamps.

Estimate the upper bound before shipping:

```text
series = methods × normalized_routes × status_classes × regions × instances
```

If one dimension has no finite, reviewed bound, it does not belong in metric labels. Put high-cardinality correlation data in sampled traces or structured logs.

## Runtime metrics

The standard `runtime/metrics` package exposes runtime values without choosing a monitoring library:

```go
descriptions := metrics.All()
samples := make([]metrics.Sample, len(descriptions))
for i, description := range descriptions {
	samples[i].Name = description.Name
}
metrics.Read(samples)
```

Metric availability changes with the Go version. Discover names with `metrics.All()` and map only the values needed by the project's exporter. Useful themes include heap classes, allocation rate, GC cycles and pauses, goroutine counts, scheduler latency, and mutex wait time.

## HTTP instrumentation boundary

Instrument normalized routes, not raw paths. Capture status after the handler runs and count panics at the recovery boundary. Keep the instrumentation interface local so the selected backend does not spread through business packages.

```go
type RequestRecorder interface {
	Observe(method, route string, status int, elapsed time.Duration)
}
```

## Validation

Before release:

1. exercise success, client error, server error, cancellation, and panic paths;
2. confirm each request contributes exactly once;
3. inspect the number of emitted series under representative routes;
4. verify labels contain no secrets or personal data;
5. confirm dashboards and alerts use the same units and normalized dimensions.
