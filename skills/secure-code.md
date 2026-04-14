# Secure Code Skill

Security hardening workflow for common code patterns. Apply before committing any security-sensitive code.

---

## When to Use

- Writing Dockerfiles
- Writing shell scripts
- Creating API endpoints
- Database queries
- Handling user input
- Managing secrets
- Error handling

---

## Decision Tree

```
What are you working on?
│
├── Dockerfile ──────────────► Go to "Dockerfile Hardening"
├── Shell script ────────────► Go to "Shell Script Hardening"
├── API endpoint ────────────► Go to "API Security"
├── Database query ──────────► Go to "Database Security"
├── User input handling ─────► Go to "Input Validation"
├── Secrets/credentials ─────► Go to "Secrets Management"
└── Error handling ──────────► Go to "Error Message Security"
```

---

## Dockerfile Hardening

### Checklist

- [ ] Use specific version tags (not `latest`)
- [ ] Run as non-root user
- [ ] Use multi-stage builds
- [ ] Minimize installed packages
- [ ] Don't store secrets in image
- [ ] Use `.dockerignore`

### Pattern

```dockerfile
# Good: Specific version, non-root, multi-stage
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app/server ./cmd/server

FROM gcr.io/distroless/static-debian12:nonroot
COPY --from=builder /app/server /server
USER nonroot:nonroot
ENTRYPOINT ["/server"]
```

### Anti-Patterns

```dockerfile
# Bad: Latest tag, root user, secrets in build
FROM ubuntu:latest
RUN apt-get update && apt-get install -y everything
ENV API_KEY=secret123
```

---

## Shell Script Hardening

### Checklist

- [ ] Use `set -euo pipefail`
- [ ] Quote all variables: `"${VAR}"`
- [ ] Validate inputs before use
- [ ] Use `[[ ]]` instead of `[ ]`
- [ ] Avoid `eval`
- [ ] Use absolute paths for commands

### Pattern

```bash
#!/usr/bin/env bash
set -euo pipefail

# Validate inputs
if [[ -z "${INPUT:-}" ]]; then
    echo "Error: INPUT required" >&2
    exit 1
fi

# Quote variables
/usr/bin/process --input "${INPUT}"
```

### Anti-Patterns

```bash
# Bad: No error handling, unquoted variables
#!/bin/bash
cd $DIR
process $INPUT
```

---

## API Security

### Checklist

- [ ] Validate all inputs
- [ ] Use authentication
- [ ] Check authorization
- [ ] Rate limiting
- [ ] Timeout contexts
- [ ] Safe error responses

### Pattern

```go
func (s *Server) CreateResource(ctx context.Context, req *pb.Request) (*pb.Response, error) {
    // 1. Timeout context
    ctx, cancel := context.WithTimeout(ctx, 30*time.Second)
    defer cancel()
    
    // 2. Input validation
    if err := validateRequest(req); err != nil {
        return nil, status.Error(codes.InvalidArgument, "invalid request")
    }
    
    // 3. Authorization check
    if !s.authz.CanCreate(ctx, req.ResourceType) {
        return nil, status.Error(codes.PermissionDenied, "not authorized")
    }
    
    // 4. Business logic with safe errors
    result, err := s.service.Create(ctx, req)
    if err != nil {
        log.Error("create failed", "error", err)  // Log details
        return nil, status.Error(codes.Internal, "operation failed")  // Safe response
    }
    
    return result, nil
}
```

---

## Database Security

### Checklist

- [ ] Parameterized queries only
- [ ] Never concatenate SQL strings
- [ ] Use least-privilege connections
- [ ] Set query timeouts
- [ ] Close connections properly

### Pattern

```go
// Good: Parameterized query
func (r *Repo) GetUser(ctx context.Context, id string) (*User, error) {
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    
    row := r.db.QueryRowContext(ctx,
        "SELECT id, name, email FROM users WHERE id = $1", id)
    
    var u User
    if err := row.Scan(&u.ID, &u.Name, &u.Email); err != nil {
        return nil, fmt.Errorf("scan user: %w", err)
    }
    return &u, nil
}
```

### Anti-Patterns

```go
// NEVER: SQL string concatenation
query := "SELECT * FROM users WHERE id = '" + userInput + "'"

// NEVER: fmt.Sprintf for SQL
query := fmt.Sprintf("SELECT * FROM users WHERE name = '%s'", name)
```

---

## Input Validation

### Checklist

- [ ] Validate at system boundary
- [ ] Whitelist allowed values
- [ ] Sanitize before use
- [ ] Limit lengths
- [ ] Type-check

### Pattern

```go
func validateEmail(email string) error {
    if email == "" {
        return errors.New("email required")
    }
    if len(email) > 254 {
        return errors.New("email too long")
    }
    if !emailRegex.MatchString(email) {
        return errors.New("invalid email format")
    }
    return nil
}
```

---

## Secrets Management

### Checklist

- [ ] Secrets from environment only
- [ ] Never hardcode credentials
- [ ] Never log secrets
- [ ] Use secret managers in production
- [ ] Rotate credentials regularly

### Pattern

```go
// Good: From environment
apiKey := os.Getenv("API_KEY")
if apiKey == "" {
    log.Fatal("API_KEY not set")
}

// Good: Secret manager
secret, err := secretManager.GetSecret(ctx, "api-key")
```

### Anti-Patterns

```go
// NEVER: Hardcoded secrets
const apiKey = "sk-12345abcde"

// NEVER: Secrets in logs
log.Info("connecting", "api_key", apiKey)
```

---

## Error Message Security

### Checklist

- [ ] Don't expose internal details
- [ ] Don't show stack traces to users
- [ ] Don't reveal database structure
- [ ] Don't leak file paths
- [ ] Log full details server-side

### Pattern

```go
// Good: Safe error to user, detailed log
if err != nil {
    log.Error("database error", 
        "error", err,
        "query", "get_user",
        "user_id", userID)
    return nil, status.Error(codes.Internal, "operation failed")
}
```

### Anti-Patterns

```go
// NEVER: Internal details to user
return nil, fmt.Errorf("SQL error: %v at /app/db/users.go:42", err)
```

---

## Quick Reference

| Area | Rule |
|------|------|
| SQL | Parameterized queries only |
| Secrets | Environment/secret manager only |
| Errors | Log details, return generic |
| Input | Validate at boundary |
| Docker | Non-root, pinned versions |
| Shell | Quote variables, set -euo pipefail |
| API | Timeout context, authz check |
