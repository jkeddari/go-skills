# Performance Investigation Session

An investigation session collects temporary, higher-resolution evidence for one defined symptom. Keep it bounded in scope and duration, and return every diagnostic setting to its previous state afterward.

## Prepare

Record:

- affected version, environment, architecture, and Go version;
- the user-visible metric and time window;
- representative input or traffic shape;
- recent deploys and configuration changes;
- current CPU, memory, goroutine, GC, queue, and dependency signals.

Choose one representative instance when fleet-wide diagnostics would multiply overhead.

## Collect in increasing cost order

1. Existing metrics and structured logs.
2. Goroutine or heap snapshots.
3. A bounded CPU, mutex, or block profile.
4. A short execution trace.
5. Extra debug logging or experimental runtime flags only when cheaper evidence cannot discriminate the hypothesis.

Protect diagnostic endpoints and artifacts. Profiles and traces can expose code paths, filenames, arguments, and internal topology.

## Runtime samples

Use `runtime/metrics` to observe allocation, GC, scheduling, and contention themes. See [benchmark-runtime-metrics.md](benchmark-runtime-metrics.md). Export through the monitoring boundary already present in the project; an investigation should not introduce a new application dependency.

## Evidence log

For every capture, write down:

```text
timestamp:
version and configuration:
workload:
artifact or command:
hypothesis tested:
observation:
next decision:
```

This prevents a sequence of expensive captures from becoming an untraceable collection of screenshots and profile files.

## Cost controls

- CPU profiling changes CPU availability during capture.
- Mutex and block profiling add event-recording overhead.
- Execution traces grow quickly; use the shortest window that includes the symptom.
- Debug logs allocate and perform I/O, which can alter the behavior under study.
- High-frequency metric collection has its own CPU, allocation, network, and storage cost.

Measure diagnostic overhead where possible. Disable temporary settings and remove exposed endpoints when the session ends.

## Exit criteria

Finish with a source-level or system-level hypothesis supported by evidence, a focused benchmark or reproduction, and an expected metric change. If evidence remains ambiguous, state what is still unknown rather than optimizing the most visually prominent profile frame.
