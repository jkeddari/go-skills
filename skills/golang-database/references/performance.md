# Database Performance

Treat database performance as a system property: query shape, indexes, transaction duration, pool limits, network latency, and concurrent application demand interact. Measure the real bottleneck before tuning a single layer.

## Connection pool

`database/sql` is already a concurrency-safe pool:

```go
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(10)
db.SetConnMaxLifetime(5 * time.Minute)
db.SetConnMaxIdleTime(time.Minute)
```

Derive limits from database capacity and total application replicas. A generous per-process limit multiplied across a deployment can overwhelm the server.

Inspect `db.Stats()` over time:

```go
stats := db.Stats()
slog.Info("database pool",
	"open", stats.OpenConnections,
	"in_use", stats.InUse,
	"idle", stats.Idle,
	"wait_count", stats.WaitCount,
	"wait_duration", stats.WaitDuration,
)
```

Increasing `WaitCount` means callers waited for a connection; it does not by itself prove the pool is too small. Slow queries, long transactions, leaked rows, or database saturation can create the same symptom.

## Query workflow

1. Capture the exact parameterized query and representative parameter distribution.
2. Measure latency and rows examined or returned.
3. Inspect the database execution plan using the target database's safe plan command.
4. Confirm existing indexes and table statistics.
5. Change one query or index hypothesis and compare under representative concurrency.

Do not run mutating plan variants or create indexes on production without explicit operational approval. Indexes speed some reads but consume memory and storage and add write amplification.

## Avoid N+1 work

Repeated queries inside a loop add network round trips and pool contention. Prefer a bounded batch query or a join when it preserves semantics. Keep batch size below driver and database parameter limits, and benchmark with realistic row widths.

## Batch writes

Avoid both one round trip per row and an unbounded transaction. Chunk work with a measured batch size:

```go
const batchSize = 500
for start := 0; start < len(items); start += batchSize {
	end := min(start+batchSize, len(items))
	if err := insertBatch(ctx, db, items[start:end]); err != nil {
		return fmt.Errorf("insert batch %d:%d: %w", start, end, err)
	}
}
```

The correct size depends on row width, parameter count, lock duration, log volume, and server capacity. Make cancellation and partial-failure semantics explicit.

## Pagination

Large offsets force the database to find and discard earlier rows. Prefer keyset pagination over a stable, indexed, unique ordering:

```sql
SELECT id, created_at, payload
FROM events
WHERE (created_at, id) > (?, ?)
ORDER BY created_at, id
LIMIT ?;
```

Include a unique tie-breaker so items with the same timestamp are neither skipped nor duplicated. Document how deletions and concurrent inserts affect traversal.

## Verification

- query latency distribution and error rate;
- rows returned versus rows examined;
- pool wait count and duration;
- transaction duration and lock waits;
- database CPU, I/O, connections, and cache behavior;
- correctness under cancellation, retries, and concurrent updates.

→ See `golang-performance` for comparison discipline and `golang-observability` for signal design.
