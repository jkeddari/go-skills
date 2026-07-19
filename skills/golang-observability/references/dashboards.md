# Dashboard Design

A dashboard should support a decision, not display every available metric. Organize it from user impact to likely causes so an operator can narrow a problem without switching mental models.

## Service overview

Use the same time range and deployment annotations across panels:

1. traffic or job throughput;
2. success and error rate;
3. latency distribution, especially p95 and p99;
4. saturation of queues, workers, pools, CPU, and memory;
5. active version, region, and instance count.

## Runtime drill-down

Add runtime panels only when they help explain service behavior:

- goroutines and threads;
- heap live bytes, allocation rate, and heap goal;
- GC cycle rate and pause distribution;
- scheduler latency;
- mutex or block contention when measured;
- process CPU and resident memory.

Correlate these with request volume and deploys. A rising goroutine count is meaningful only after ruling out increased traffic or a planned worker change.

## Design rules

- Put units in titles and use consistent scales.
- Show rates for cumulative counters.
- Prefer normalized routes and bounded dimensions.
- Annotate deploys and configuration changes.
- Include links to runbooks and the owning service.
- Avoid red/green thresholds unless the threshold represents a reviewed objective.
- Test empty, partial, and stale-data states so missing telemetry is not mistaken for health.

Use reusable dashboard templates only after reviewing their queries, units, label assumptions, and cardinality against the application's own instrumentation.
