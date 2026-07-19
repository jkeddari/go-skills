# CI Benchmark Regression Detection

Benchmark gates are useful only when the environment is stable enough to distinguish a code change from noise. Shared runners, thermal throttling, CPU-frequency changes, and background load can easily produce false regressions.

## Baseline workflow

1. Select a small set of benchmarks tied to an explicit performance requirement.
2. Run baseline and candidate on the same runner class with identical Go version and environment.
3. Use multiple samples and record raw output as artifacts.
4. Compare distributions with `benchstat`.
5. Gate only on changes large enough to matter and stable enough to reproduce.

```bash
go test -run '^$' -bench 'BenchmarkCritical' -benchmem -count 10 ./path/... > candidate.txt
benchstat base.txt candidate.txt
```

For a pull request, obtain `base.txt` from a trusted baseline artifact or build the base revision in a separate clean worktree. Never reset the developer's active worktree as part of comparison automation.

## Environment record

Store these with each result:

- commit and dirty state;
- Go version and experiment flags;
- operating system and architecture;
- CPU model and runner class;
- `GOMAXPROCS` and relevant runtime settings;
- benchmark flags, sample count, and duration;
- package and benchmark names.

## Gate policy

- Require statistical evidence and a practical effect size.
- Prefer a human-reviewed report before enabling failure gates.
- Separate latency, throughput, bytes, and allocations; improvement in one can regress another.
- Re-run suspicious changes on a dedicated runner before treating them as defects.
- Update the baseline intentionally after an accepted tradeoff; do not silently overwrite it on every run.

A trend dashboard can help discover slow drift, but it does not replace a controlled comparison or a profile that explains the source-level cause.
