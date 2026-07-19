# Production Signals During Performance Work

Benchmarks and profiles explain a controlled sample; production telemetry shows whether the symptom exists under real load. Use both, but keep their ownership distinct from always-on observability design.

## Establish the production symptom

Correlate the affected version and load with:

- throughput and error rate;
- latency distribution rather than average latency;
- process CPU and resident memory;
- allocation rate, live heap, GC cycles, and pauses;
- goroutine count and scheduler latency;
- queue depth, worker saturation, and connection-pool wait;
- external dependency latency and error rate.

Compare like-for-like windows. A deployment that handles more traffic may consume more resources while becoming more efficient per request.

## Runtime sampling without a vendor

`runtime/metrics` provides in-process runtime samples. `runtime/pprof`, `net/http/pprof`, and `runtime/trace` provide temporary deep evidence. Keep capture bounded and secure, and label every artifact with version, environment, duration, and workload.

## Before and after

For a proposed optimization:

1. define the production metric expected to move;
2. record a stable baseline window;
3. deploy with a controlled comparison when possible;
4. verify correctness and resource tradeoffs, not only the target metric;
5. retain a benchmark or alert only when it detects a meaningful regression.

→ See `golang-observability` for exporter, dashboard, and alert design.
