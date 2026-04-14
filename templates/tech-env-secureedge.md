# Technical Environment: SecureEdge Services

> **Pre-filled template for SecureEdge brownfield work** — Copy to
> `workshop/inputs/<team>-tech-env.md` and customize as needed.

---

## Existing Stack

| Component | Technology | Version | Notes |
|-----------|------------|---------|-------|
| **Language** | Go | 1.24 | |
| **API Framework** | gRPC + gRPC-Gateway | | In-process gateway |
| **Database** | PostgreSQL | 15+ | CockroachDB compatible |
| **Cache** | Redis/Valkey | | |
| **Message Queue** | Azure Service Bus | | |
| **Observability** | OpenTelemetry | | Traces, metrics, logs |
| **Deployment** | Kubernetes (Helm) | | |
| **Container Runtime** | Docker | | Non-root containers |

---

## Code Patterns

### Project Structure

```
cmd/
  server/
    main.go              # Entry point
internal/
  service/               # Business logic
  db/                    # Database layer
  handler/               # gRPC handlers
api/
  proto/                 # Protocol buffer definitions
  openapi/               # Generated OpenAPI specs
```

### Example: gRPC Handler

```go
func (s *Server) GetDevice(ctx context.Context, req *pb.GetDeviceRequest) (*pb.GetDeviceResponse, error) {
    ctx, span := otel.Tracer("").Start(ctx, "GetDevice")
    defer span.End()

    if req.GetDeviceId() == "" {
        return nil, status.Error(codes.InvalidArgument, "device_id is required")
    }

    device, err := s.deviceService.GetDevice(ctx, req.GetDeviceId())
    if err != nil {
        if errors.Is(err, ErrNotFound) {
            return nil, status.Error(codes.NotFound, "device not found")
        }
        return nil, status.Error(codes.Internal, "internal error")
    }

    return &pb.GetDeviceResponse{Device: device.ToProto()}, nil
}
```

### Example: Service Layer

```go
func (s *DeviceService) GetDevice(ctx context.Context, deviceID string) (*Device, error) {
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()

    device, err := s.repo.GetDevice(ctx, deviceID)
    if err != nil {
        return nil, fmt.Errorf("get device: %w", err)
    }

    return device, nil
}
```

### Example: Database Query

```go
func (r *Repository) GetDevice(ctx context.Context, deviceID string) (*Device, error) {
    query := `SELECT id, name, status FROM devices WHERE id = $1`
    
    var device Device
    err := r.db.QueryRowContext(ctx, query, deviceID).Scan(&device.ID, &device.Name, &device.Status)
    if err != nil {
        if errors.Is(err, sql.ErrNoRows) {
            return nil, ErrNotFound
        }
        return nil, fmt.Errorf("query device: %w", err)
    }

    return &device, nil
}
```

### Example: Unit Test

```go
func TestDeviceService_GetDevice(t *testing.T) {
    t.Run("returns device when found", func(t *testing.T) {
        repo := &mockRepo{
            device: &Device{ID: "123", Name: "test"},
        }
        svc := NewDeviceService(repo)

        device, err := svc.GetDevice(context.Background(), "123")

        require.NoError(t, err)
        assert.Equal(t, "123", device.ID)
    })

    t.Run("returns error when not found", func(t *testing.T) {
        repo := &mockRepo{err: ErrNotFound}
        svc := NewDeviceService(repo)

        _, err := svc.GetDevice(context.Background(), "123")

        assert.ErrorIs(t, err, ErrNotFound)
    })
}
```

---

## Prohibited Patterns

| Pattern | Reason | Use Instead |
|---------|--------|-------------|
| `fmt.Sprintf` for SQL | SQL injection risk | Parameterized queries with `$1`, `$2` |
| Global variables | Testing difficulty, race conditions | Dependency injection |
| `panic()` in handlers | Service availability | Return errors |
| `log.Fatal()` in handlers | Unclean shutdown | Return errors, log at top level |
| Hardcoded secrets | Security | Environment variables / K8s secrets |
| `time.Sleep()` in tests | Flaky tests | Channels, condition variables |
| `interface{}` / `any` | Type safety | Concrete types or generics |
| Unbounded queries | Memory exhaustion | Pagination with LIMIT/OFFSET |

---

## Prohibited Libraries

| Library | Reason | Use Instead |
|---------|--------|-------------|
| `github.com/sirupsen/logrus` | Not structured | `log/slog` |
| `github.com/go-kit/log` | Deprecated | `log/slog` |
| `github.com/pkg/errors` | Deprecated | `fmt.Errorf` with `%w` |
| `github.com/stretchr/testify/suite` | Complex | Table-driven tests with `t.Run` |

---

## Security Requirements

### Authentication
- mTLS for service-to-service
- JWT tokens for user authentication
- Validate tokens on every request

### Authorization
- Check permissions before any data access
- Tenant isolation enforced at query level

### Input Validation
- Validate all external inputs
- Use allowlists, not denylists
- Sanitize before logging (no PII in logs)

### Secrets Management
- Environment variables for secrets
- Never hardcode credentials
- Never log secrets

### Error Handling
- Don't leak internal details in error messages
- Log full errors server-side
- Return sanitized errors to clients

---

## Testing Requirements

| Type | Coverage Target | Framework |
|------|-----------------|-----------|
| Unit Tests | 80% on new code | `testing` + `testify` |
| Integration Tests | Critical paths | `testing` + real DB |
| E2E Tests | Happy paths | Separate repo |

### Test Commands

```bash
# Run all tests
make test

# Run with coverage
make compose-test-coverage

# Run specific package
go test ./internal/service/...
```

---

## Build & Deploy

### Local Development

```bash
# Start dependencies
make compose-up

# Build and run
make build run

# Run linters
make lint lint-fix
```

### CI/CD

```bash
# Full quality check
go test ./...
make lint
govulncheck ./...
go mod verify
```

### Ports

| Port | Service |
|------|---------|
| 50051 | gRPC |
| 8089 | REST (gRPC-Gateway) |
| 8081 | Health checks |

---

## What This Team Will Add

> Customize this section for your specific feature

### New Components
- [ ] 

### Modified Components
- [ ] 

### New APIs
- [ ] 

### Database Changes
- [ ] 

---

## References

- [Architecture Overview](../../docs/dev/architecture.md)
- [Code Conventions](../../docs/dev/code-conventions.md)
- [Observability Guide](../../docs/dev/observability.md)
- [API Documentation](../../api/README.md)
