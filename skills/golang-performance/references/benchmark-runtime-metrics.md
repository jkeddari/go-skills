# Go Runtime Metrics for Investigations

The standard `runtime/metrics` package exposes a versioned set of runtime samples. Use it to connect a benchmark or production symptom to allocation, garbage collection, scheduling, and contention behavior without coupling the investigation to a monitoring library.

## Discover available samples

```go
package main

import (
	"fmt"
	"runtime/metrics"
)

func main() {
	descriptions := metrics.All()
	samples := make([]metrics.Sample, len(descriptions))
	for i, description := range descriptions {
		samples[i].Name = description.Name
	}
	metrics.Read(samples)

	for i, sample := range samples {
		fmt.Printf("%s (%s): %v\n", sample.Name, descriptions[i].Unit, sample.Value)
	}
}
```

Do not hardcode a catalog from another Go version. Use `metrics.All()` in the target toolchain and choose samples by documented name and unit.

## Investigation themes

| Symptom | Runtime evidence | Follow-up |
| --- | --- | --- |
| rising memory | live heap classes, heap goal, allocation bytes and objects | compare heap profiles and object lifetimes |
| frequent GC | GC cycle counters and allocation rate | reduce allocations before tuning GC |
| tail latency | GC pauses and scheduler latency histograms | correlate timestamps with request latency |
| growing concurrency | goroutine counts and runnable work | inspect goroutine profiles and queue bounds |
| lock contention | mutex wait CPU time | capture mutex profile under representative load |

## Reading histograms

Runtime histograms expose bucket boundaries and cumulative counts. Compare distributions with the same Go version and workload; changing runtime versions can change buckets or scheduler behavior. Preserve raw counts when exporting them so downstream systems can calculate quantiles consistently.

## Measurement discipline

- Record the Go version, architecture, `GOMAXPROCS`, environment, and workload.
- Compare rates over equal durations instead of raw cumulative counters.
- Treat runtime samples as correlation evidence, not proof of a source-level cause.
- Confirm the suspected source with pprof, trace, compiler diagnostics, or a focused benchmark.
