# Deployment Checklist

> **Team**: [Team Name]  
> **Feature**: [Feature Name]  
> **Target Environment**: staging / production  
> **Deployment Date**: [Date]

---

## Pre-Deployment

### Code Readiness

- [ ] All PRs merged to main/release branch
- [ ] Production readiness checklist complete
- [ ] Version tagged in git
- [ ] Changelog updated

### Configuration

- [ ] Environment variables identified
  ```
  NEW_FEATURE_ENABLED=true
  NEW_SERVICE_URL=https://...
  ```
- [ ] Secrets created in target environment
- [ ] ConfigMaps updated (if applicable)

### Database

- [ ] Migration scripts ready
- [ ] Migration tested in staging
- [ ] Estimated migration duration: [X minutes]
- [ ] Maintenance window needed: Yes / No
- [ ] Rollback migration ready

### Dependencies

- [ ] Downstream services notified
- [ ] Upstream services compatible
- [ ] Third-party dependencies available
- [ ] API version compatibility verified

---

## Helm Chart Changes

### Values Updated

| File | Change | Reviewed |
|------|--------|----------|
| `values.yaml` | | ☐ |
| `values-staging.yaml` | | ☐ |
| `values-production.yaml` | | ☐ |

### New Resources

- [ ] Deployments
- [ ] Services
- [ ] ConfigMaps
- [ ] Secrets
- [ ] Ingress rules
- [ ] ServiceAccounts
- [ ] RBAC

### Resource Requests

```yaml
resources:
  requests:
    cpu: 
    memory: 
  limits:
    cpu: 
    memory: 
```

---

## Feature Flag Configuration

### SplitIO Setup

- [ ] Feature flag created: `[flag-name]`
- [ ] Default treatment: `off`
- [ ] Targeting rules configured

### Rollout Plan

| Phase | Percentage | Duration | Success Criteria |
|-------|------------|----------|------------------|
| 1 | 5% | 1 hour | No errors |
| 2 | 25% | 4 hours | Error rate < 0.1% |
| 3 | 50% | 1 day | Latency p99 < 200ms |
| 4 | 100% | - | All metrics stable |

### Kill Switch

- [ ] Kill switch tested
- [ ] Rollback time: [X minutes]
- [ ] On-call knows how to disable

---

## Deployment Steps

### Staging

```bash
# 1. Deploy to staging
helm upgrade --install [release] ./helm \
  -f helm/values-staging.yaml \
  --namespace [namespace]

# 2. Run smoke tests
make test-staging

# 3. Verify metrics
# Check Grafana dashboard: [URL]

# 4. Test feature flag
# Enable for test accounts in SplitIO
```

### Production

```bash
# 1. Notify on-call
# Slack: #[channel]

# 2. Create deployment ticket
# Jira: [ticket]

# 3. Deploy with feature flag OFF
helm upgrade --install [release] ./helm \
  -f helm/values-production.yaml \
  --namespace [namespace]

# 4. Verify pods healthy
kubectl get pods -n [namespace] -l app=[app]

# 5. Run smoke tests
make test-production-smoke

# 6. Begin rollout via feature flag
# SplitIO: Enable 5% traffic
```

---

## Monitoring During Rollout

### Key Metrics to Watch

| Metric | Baseline | Alert Threshold | Dashboard |
|--------|----------|-----------------|-----------|
| Error rate | | | |
| Latency p99 | | | |
| CPU usage | | | |
| Memory usage | | | |

### Log Queries

```
# Errors from new feature
service="[service]" level="error" feature="[feature]"

# Latency outliers
service="[service]" duration_ms > 500
```

### Dashboards

- [ ] [Dashboard Name](grafana-url)
- [ ] [Dashboard Name](grafana-url)

---

## Rollback Procedure

### Automatic Rollback Triggers

- [ ] Error rate > [X]%
- [ ] Latency p99 > [X]ms
- [ ] Pod crash loops

### Manual Rollback Steps

```bash
# Option 1: Disable feature flag (immediate)
# SplitIO: Set flag to 'off' for all

# Option 2: Helm rollback (if config issue)
helm rollback [release] [revision] -n [namespace]

# Option 3: Full rollback (if code issue)
helm upgrade --install [release] ./helm \
  -f helm/values-production.yaml \
  --set image.tag=[previous-version] \
  --namespace [namespace]
```

### Database Rollback (if needed)

```bash
# Run down migration
make migrate-down VERSION=[version]
```

---

## Post-Deployment

### Verification

- [ ] All pods healthy
- [ ] Smoke tests pass
- [ ] Feature flag rollout at target percentage
- [ ] No error spikes
- [ ] No latency degradation

### Communication

- [ ] Deployment complete notification sent
- [ ] Release notes published
- [ ] Support team notified

### Cleanup

- [ ] Remove old feature flags (after full rollout)
- [ ] Archive deployment ticket
- [ ] Update documentation

---

## Contacts

| Role | Name | Contact |
|------|------|---------|
| Deployer | | |
| On-Call | | |
| Tech Lead | | |
| Product | | |

---

## Notes

[Deployment-specific notes, known issues, or special instructions]
