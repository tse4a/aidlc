# Database Security Hardening

Comprehensive guide to secure database access.

## Core Principles

1. **Parameterized queries only** — Never concatenate SQL
2. **Least privilege** — Minimal permissions
3. **Timeouts** — Prevent runaway queries
4. **Connection security** — Encrypt in transit

---

## Parameterized Queries

### The Golden Rule

**NEVER concatenate user input into SQL strings.**

```go
// ❌ VULNERABLE: SQL injection
func (r *Repo) GetUser(id string) (*User, error) {
    query := "SELECT * FROM users WHERE id = '" + id + "'"
    return r.db.Query(query)
}

// ❌ ALSO VULNERABLE: fmt.Sprintf
func (r *Repo) GetUser(id string) (*User, error) {
    query := fmt.Sprintf("SELECT * FROM users WHERE id = '%s'", id)
    return r.db.Query(query)
}

// ✅ SAFE: Parameterized query
func (r *Repo) GetUser(ctx context.Context, id string) (*User, error) {
    query := "SELECT id, name, email FROM users WHERE id = $1"
    row := r.db.QueryRowContext(ctx, query, id)
    
    var u User
    if err := row.Scan(&u.ID, &u.Name, &u.Email); err != nil {
        return nil, err
    }
    return &u, nil
}
```

### Parameter Placeholders by Database

| Database | Placeholder |
|----------|-------------|
| PostgreSQL | `$1`, `$2`, `$3` |
| MySQL | `?` |
| SQLite | `?` or `$1` |
| SQL Server | `@p1`, `@p2` |

---

## Query Timeouts

### Always Use Context

```go
func (r *Repo) GetUsers(ctx context.Context) ([]User, error) {
    // Create timeout context if not already set
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    
    rows, err := r.db.QueryContext(ctx,
        "SELECT id, name, email FROM users")
    if err != nil {
        return nil, fmt.Errorf("query users: %w", err)
    }
    defer rows.Close()
    
    var users []User
    for rows.Next() {
        var u User
        if err := rows.Scan(&u.ID, &u.Name, &u.Email); err != nil {
            return nil, fmt.Errorf("scan user: %w", err)
        }
        users = append(users, u)
    }
    
    return users, rows.Err()
}
```

### Statement-Level Timeouts

```sql
-- PostgreSQL
SET statement_timeout = '5s';

-- MySQL
SET max_execution_time = 5000;
```

---

## Connection Pooling

### Configure Limits

```go
func NewDB(dsn string) (*sql.DB, error) {
    db, err := sql.Open("postgres", dsn)
    if err != nil {
        return nil, err
    }
    
    // Connection pool settings
    db.SetMaxOpenConns(25)           // Max total connections
    db.SetMaxIdleConns(5)            // Keep some ready
    db.SetConnMaxLifetime(5*time.Minute)  // Recycle connections
    db.SetConnMaxIdleTime(1*time.Minute)  // Close idle connections
    
    // Verify connection
    ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
    defer cancel()
    
    if err := db.PingContext(ctx); err != nil {
        return nil, fmt.Errorf("ping database: %w", err)
    }
    
    return db, nil
}
```

---

## Least Privilege

### Create Limited Users

```sql
-- Application user: read/write to specific tables
CREATE USER app_user WITH PASSWORD 'secure_password';
GRANT SELECT, INSERT, UPDATE ON users, orders TO app_user;

-- Read-only user for reporting
CREATE USER report_user WITH PASSWORD 'secure_password';
GRANT SELECT ON users, orders TO report_user;

-- Migration user: schema changes only
CREATE USER migrate_user WITH PASSWORD 'secure_password';
GRANT ALL ON SCHEMA public TO migrate_user;
```

### Row-Level Security

```sql
-- Enable RLS
ALTER TABLE documents ENABLE ROW LEVEL SECURITY;

-- Users can only see their own documents
CREATE POLICY user_documents ON documents
    FOR ALL
    USING (owner_id = current_setting('app.user_id')::uuid);
```

---

## Input Validation

### Validate Before Querying

```go
func (r *Repo) GetUserByEmail(ctx context.Context, email string) (*User, error) {
    // Validate email format
    if !isValidEmail(email) {
        return nil, fmt.Errorf("invalid email format")
    }
    
    // Now safe to query (still parameterized!)
    row := r.db.QueryRowContext(ctx,
        "SELECT id, name, email FROM users WHERE email = $1",
        email)
    
    var u User
    if err := row.Scan(&u.ID, &u.Name, &u.Email); err != nil {
        return nil, err
    }
    return &u, nil
}
```

### Whitelist for Dynamic Queries

```go
var allowedSortColumns = map[string]bool{
    "name":       true,
    "created_at": true,
    "updated_at": true,
}

func (r *Repo) ListUsers(ctx context.Context, sortBy string) ([]User, error) {
    // Whitelist check for dynamic column
    if !allowedSortColumns[sortBy] {
        sortBy = "created_at"  // Default to safe value
    }
    
    // Safe because sortBy is from whitelist
    query := fmt.Sprintf(
        "SELECT id, name FROM users ORDER BY %s",
        sortBy)
    
    return r.db.QueryContext(ctx, query)
}
```

---

## Transaction Safety

### Use Transactions for Multi-Step Operations

```go
func (r *Repo) TransferFunds(ctx context.Context, from, to string, amount int) error {
    tx, err := r.db.BeginTx(ctx, nil)
    if err != nil {
        return fmt.Errorf("begin tx: %w", err)
    }
    defer tx.Rollback()  // No-op if committed
    
    // Debit from account
    result, err := tx.ExecContext(ctx,
        "UPDATE accounts SET balance = balance - $1 WHERE id = $2 AND balance >= $1",
        amount, from)
    if err != nil {
        return fmt.Errorf("debit: %w", err)
    }
    
    rows, _ := result.RowsAffected()
    if rows == 0 {
        return fmt.Errorf("insufficient funds")
    }
    
    // Credit to account
    _, err = tx.ExecContext(ctx,
        "UPDATE accounts SET balance = balance + $1 WHERE id = $2",
        amount, to)
    if err != nil {
        return fmt.Errorf("credit: %w", err)
    }
    
    return tx.Commit()
}
```

---

## Connection Security

### Use TLS

```go
// PostgreSQL with TLS
dsn := "postgres://user:pass@host:5432/db?sslmode=verify-full&sslrootcert=/path/to/ca.crt"

// MySQL with TLS
dsn := "user:pass@tcp(host:3306)/db?tls=true"
```

### Environment Variables for Credentials

```go
func NewDB() (*sql.DB, error) {
    host := os.Getenv("DB_HOST")
    port := os.Getenv("DB_PORT")
    user := os.Getenv("DB_USER")
    pass := os.Getenv("DB_PASSWORD")
    name := os.Getenv("DB_NAME")
    
    dsn := fmt.Sprintf(
        "postgres://%s:%s@%s:%s/%s?sslmode=verify-full",
        user, pass, host, port, name)
    
    return sql.Open("postgres", dsn)
}
```

---

## Audit Logging

### Log Access Patterns

```go
func (r *Repo) GetSensitiveData(ctx context.Context, id string) (*Data, error) {
    userID := getUserIDFromContext(ctx)
    
    // Log access attempt
    r.auditLog.Info("sensitive data access",
        "user_id", userID,
        "data_id", id,
        "action", "read")
    
    data, err := r.doGetSensitiveData(ctx, id)
    if err != nil {
        r.auditLog.Warn("sensitive data access failed",
            "user_id", userID,
            "data_id", id,
            "error", err.Error())
        return nil, err
    }
    
    return data, nil
}
```

---

## Complete Example

```go
type UserRepo struct {
    db       *sql.DB
    auditLog *slog.Logger
}

func (r *UserRepo) GetUser(ctx context.Context, id string) (*User, error) {
    // 1. Timeout
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    
    // 2. Validate input
    if !isValidUUID(id) {
        return nil, fmt.Errorf("invalid user id format")
    }
    
    // 3. Parameterized query
    row := r.db.QueryRowContext(ctx,
        `SELECT id, name, email, created_at 
         FROM users 
         WHERE id = $1 AND deleted_at IS NULL`,
        id)
    
    // 4. Scan results
    var u User
    if err := row.Scan(&u.ID, &u.Name, &u.Email, &u.CreatedAt); err != nil {
        if errors.Is(err, sql.ErrNoRows) {
            return nil, ErrUserNotFound
        }
        return nil, fmt.Errorf("scan user: %w", err)
    }
    
    return &u, nil
}
```

---

## Checklist

Before deploying database code:

- [ ] All queries parameterized (no string concatenation)
- [ ] Context with timeout for all operations
- [ ] Connection pool configured
- [ ] Database user has minimal permissions
- [ ] Credentials from environment variables
- [ ] TLS enabled for connections
- [ ] Input validated before querying
- [ ] Transactions for multi-step operations
- [ ] Sensitive access logged
