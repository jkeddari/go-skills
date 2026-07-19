# Production Profiling Exposure

Profiles can reveal code paths, arguments, allocation patterns, filesystem names, and internal topology. Treat profiling endpoints and stored profiles as sensitive operational data.

## Standard-library capture

Use `runtime/pprof` for controlled file capture or `net/http/pprof` behind an authenticated administrative listener. Do not attach debug handlers to a public default mux.

```go
f, err := os.Create("cpu.pprof")
if err != nil {
	return err
}
defer f.Close()

if err := pprof.StartCPUProfile(f); err != nil {
	return err
}
defer pprof.StopCPUProfile()
```

Keep capture duration bounded and record the deployed version, load shape, and relevant configuration alongside the file.

## Continuous profiling policy

If the deployment uses a continuous profiler, define:

- which profile types are collected and why;
- expected CPU, memory, network, and storage overhead;
- sampling and retention periods;
- access control and data residency;
- whether capture is fleet-wide or limited to representative instances;
- how profiling is disabled during an incident caused by the profiler itself.

Start with the smallest useful profile set. Mutex and block profiling add runtime cost and should be enabled when investigating contention, not automatically everywhere.

## Verification

Measure overhead under representative load, confirm endpoint isolation, exercise enable and disable paths, and verify stored profiles follow the same access and retention controls as other sensitive telemetry.

→ See `golang-performance` for profile interpretation and `golang-troubleshooting` for pprof commands.
