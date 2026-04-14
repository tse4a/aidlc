# Go Pre-Commit Checklist

Complete checklist for Go projects.

## Commands

```bash
# Run all checks
go test ./... && \
golangci-lint run && \
govulncheck ./... && \
go mod verify
```

## Tests

```bash
# Run all tests
go test ./...

# With coverage
go test -cover ./...

# Verbose
go test -v ./...

# Specific package
go test ./internal/auth/...

# Race detection (CI)
go test -race ./...
```

**Must pass:** All tests green, no skipped tests hiding failures.

## Linting

```bash
# Full lint
golangci-lint run

# Auto-fix what's possible
golangci-lint run --fix

# Specific linters
golangci-lint run --enable=gosec,errcheck
```

### Common Issues

| Linter | Issue | Fix |
|--------|-------|-----|
| `errcheck` | Unchecked error | Handle or explicitly ignore: `_ = fn()` |
| `gosec` | Security issue | Address the vulnerability |
| `staticcheck` | Bug pattern | Follow suggestion |
| `govet` | Suspicious code | Review and fix |

## Security

```bash
# Check for known vulnerabilities
govulncheck ./...

# Verify dependencies
go mod verify

# Check for updates
go list -u -m all
```

## Formatting

```bash
# Format code
gofmt -w .

# Or use goimports (also organizes imports)
goimports -w .
```

## Module Management

```bash
# Tidy dependencies
go mod tidy

# Verify checksums
go mod verify

# Check for updates
go list -u -m all
```

## Build Verification

```bash
# Build without running
go build ./...

# Build with race detector
go build -race ./...
```

## Complete Script

```bash
#!/bin/bash
set -euo pipefail

echo "=== Formatting ==="
gofmt -w .
goimports -w .

echo "=== Testing ==="
go test ./...

echo "=== Linting ==="
golangci-lint run

echo "=== Security ==="
govulncheck ./...

echo "=== Module Verify ==="
go mod tidy
go mod verify

echo "=== Build ==="
go build ./...

echo "✅ All checks passed!"
```

## Self-Review Checklist

- [ ] No `fmt.Println` debug statements
- [ ] No `// TODO` without issue reference
- [ ] Errors wrapped with context: `fmt.Errorf("context: %w", err)`
- [ ] No hardcoded secrets
- [ ] Context passed to all I/O operations
- [ ] Resources closed: `defer file.Close()`
- [ ] Nil checks before pointer dereference
- [ ] Concurrent code uses proper synchronization
