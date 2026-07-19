# Testing database code

## Service tests with handwritten fakes

Define repository interfaces at the service that consumes them. Use a small fake with explicit function fields or stored results.

```go
type userRepository interface {
    GetByID(context.Context, int64) (User, error)
}

type fakeUserRepository struct {
    getByID func(context.Context, int64) (User, error)
}

func (f fakeUserRepository) GetByID(ctx context.Context, id int64) (User, error) {
    return f.getByID(ctx, id)
}
```

This verifies business behavior without pretending to verify SQL semantics.

## Integration tests

Run query and transaction tests against the same database engine and relevant version used in production. Gate infrastructure-dependent tests with a build tag when they should not run in the default unit suite.

```go
//go:build integration

func TestRepository_CreateAndGet(t *testing.T) {
    db := openTestDatabase(t)
    t.Cleanup(func() { _ = db.Close() })

    resetDatabase(t, db)
    repo := NewRepository(db)

    // Exercise the real query and assert persisted behavior.
}
```

Use migrations to build the test schema. Isolate tests with transaction rollback or deterministic cleanup, but do not rely on rollback when the code opens independent connections or commits internally.

## Cases to cover

- no rows and duplicate-key behavior;
- NULL conversion;
- context cancellation and deadlines;
- transaction rollback and commit failure;
- lock contention or isolation behavior when it is part of the contract;
- scanning after schema changes;
- pool exhaustion behavior for critical services.

Avoid exact SQL-string assertions unless query text is itself the contract. Prefer behavior against the real engine.
