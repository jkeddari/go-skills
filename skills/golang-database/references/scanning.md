# Row Scanning and NULL Semantics

`database/sql` scans columns in query order. Select explicit columns so schema changes cannot silently shift positional assumptions.

## One row

```go
type User struct {
	ID        int64
	Name      string
	Email     string
	DeletedAt *time.Time
}

func findUser(ctx context.Context, db *sql.DB, id int64) (User, error) {
	const query = `
		SELECT id, name, email, deleted_at
		FROM users
		WHERE id = ?`

	var (
		user      User
		deletedAt sql.NullTime
	)
	err := db.QueryRowContext(ctx, query, id).Scan(
		&user.ID,
		&user.Name,
		&user.Email,
		&deletedAt,
	)
	if err != nil {
		return User{}, fmt.Errorf("select user %d: %w", id, err)
	}
	if deletedAt.Valid {
		user.DeletedAt = &deletedAt.Time
	}
	return user, nil
}
```

Handle `sql.ErrNoRows` at the repository boundary where “missing” can be translated into a domain result. Do not turn every database error into “not found.”

## Multiple rows

```go
rows, err := db.QueryContext(ctx, query, active)
if err != nil {
	return nil, fmt.Errorf("query users: %w", err)
}
defer rows.Close()

var users []User
for rows.Next() {
	var user User
	if err := rows.Scan(&user.ID, &user.Name, &user.Email); err != nil {
		return nil, fmt.Errorf("scan user: %w", err)
	}
	users = append(users, user)
}
if err := rows.Err(); err != nil {
	return nil, fmt.Errorf("iterate users: %w", err)
}
return users, nil
```

`rows.Err()` captures failures that occur after iteration begins. Omitting it can return a partial result as if it were complete.

## NULL choices

Use a type that preserves whether the database value was NULL:

- `sql.Null[T]` or a concrete `sql.NullString`, `sql.NullTime`, and similar type at the storage boundary;
- a pointer when absence is a meaningful domain state;
- a domain-specific optional type when more than two states exist.

`COALESCE` is appropriate only when NULL and the replacement value have the same business meaning. Converting NULL to an empty string or zero can otherwise erase information.

Keep storage types out of public APIs when they leak database mechanics. Convert deliberately at the repository boundary and test NULL, zero, and non-zero cases separately.
