# NFR Design

> **Status**: Optional — Use if time permits and NFR Requirements were completed
> 
> **When to use**: After NFR Requirements, to incorporate NFR patterns into design

---

## Team Information

| Field | Value |
|-------|-------|
| **Team Name** | |
| **Feature** | |
| **Unit** | (if applicable) |

---

## Performance Patterns

### Caching Strategy

| Cache | Type | TTL | Invalidation |
|-------|------|-----|--------------|
| | Local / Distributed | | |
| | | | |

### Query Optimization

| Query | Current | Optimized | Improvement |
|-------|---------|-----------|-------------|
| | | | |

### Connection Pooling

| Resource | Pool Size | Timeout |
|----------|-----------|---------|
| Database | | |
| HTTP Client | | |
| Redis | | |

---

## Scalability Patterns

### Horizontal Scaling

- [ ] Stateless service design
- [ ] Session externalization
- [ ] Distributed cache

### Load Distribution

| Component | Strategy |
|-----------|----------|
| API Gateway | |
| Service | |
| Database | |

---

## Resilience Patterns

### Circuit Breaker

| Dependency | Threshold | Timeout | Fallback |
|------------|-----------|---------|----------|
| | | | |

### Retry Strategy

| Operation | Max Retries | Backoff | Idempotent |
|-----------|-------------|---------|------------|
| | | Exponential / Linear | Yes / No |

### Timeout Configuration

| Operation | Timeout | Rationale |
|-----------|---------|-----------|
| | | |

---

## Security Patterns

### Input Validation

| Input | Validation | Sanitization |
|-------|------------|--------------|
| | | |

### Rate Limiting

| Endpoint | Limit | Window | Action |
|----------|-------|--------|--------|
| | | | |

---

## Observability Patterns

### Metrics

| Metric | Type | Labels |
|--------|------|--------|
| | Counter / Histogram / Gauge | |

### Distributed Tracing

| Span | Attributes |
|------|------------|
| | |

---

## Design Decisions

| NFR | Pattern Applied | Implementation |
|-----|-----------------|----------------|
| | | |
| | | |

---

## Notes

[Pattern rationale, trade-offs, alternatives considered]
