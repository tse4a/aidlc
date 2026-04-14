# Infrastructure Design

> **Status**: Optional — Use if time permits and feature has infrastructure needs
> 
> **When to use**: New services requiring deployment infrastructure, database changes,
> external service integrations, Kubernetes resources

---

## Team Information

| Field | Value |
|-------|-------|
| **Team Name** | |
| **Feature** | |
| **Unit** | (if applicable) |

---

## Deployment Architecture

### Service Topology

```
[Diagram placeholder - describe service layout]

Example:
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Ingress   │────▶│   Service   │────▶│  Database   │
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │    Redis    │
                    └─────────────┘
```

### Environments

| Environment | Purpose | Differences from Prod |
|-------------|---------|----------------------|
| Dev | | |
| QA | | |
| Staging | | |
| Production | | |

---

## Kubernetes Resources

### Deployments

| Resource | Replicas | CPU Request | Memory Request | CPU Limit | Memory Limit |
|----------|----------|-------------|----------------|-----------|--------------|
| | | | | | |

### Services

| Service | Type | Ports | Notes |
|---------|------|-------|-------|
| | ClusterIP / LoadBalancer | | |

### ConfigMaps & Secrets

| Name | Type | Contents |
|------|------|----------|
| | ConfigMap | |
| | Secret | |

### Ingress

| Path | Service | TLS | Auth |
|------|---------|-----|------|
| | | Yes / No | |

---

## Database Infrastructure

### Schema Changes

| Change | Type | Migration Strategy |
|--------|------|-------------------|
| | Add table / Add column / Modify | Online / Offline |

### Connection Configuration

| Setting | Value | Rationale |
|---------|-------|-----------|
| Max Connections | | |
| Connection Timeout | | |
| Idle Timeout | | |

---

## External Dependencies

### Service Dependencies

| Service | Purpose | SLA | Fallback |
|---------|---------|-----|----------|
| | | | |

### API Integrations

| API | Auth Method | Rate Limits | Retry Strategy |
|-----|-------------|-------------|----------------|
| | | | |

---

## Resource Estimates

### Compute

| Component | vCPU | Memory | Storage |
|-----------|------|--------|---------|
| | | | |
| **Total** | | | |

### Cost Estimate

| Resource | Monthly Cost | Notes |
|----------|-------------|-------|
| Compute | | |
| Storage | | |
| Network | | |
| **Total** | | |

---

## Helm Values

```yaml
# Key Helm values for this feature
# (include only non-default values)

replicaCount: 2

resources:
  requests:
    cpu: 100m
    memory: 128Mi
  limits:
    cpu: 500m
    memory: 256Mi

# Add feature-specific values below
```

---

## Infrastructure Checklist

- [ ] Resource requests/limits defined
- [ ] Health checks configured (liveness/readiness)
- [ ] Horizontal Pod Autoscaler (if needed)
- [ ] Network policies defined
- [ ] Secrets management approach
- [ ] Database migration plan
- [ ] Rollback strategy documented

---

## Notes

[Infrastructure decisions, constraints, or open questions]
