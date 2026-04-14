# Production Readiness Checklist

> **Team**: [Team Name]  
> **Feature**: [Feature Name]  
> **Workshop Date**: [Date]  
> **Target Production Date**: [Date]

---

## Summary

| Category | Status | Blocking Issues |
|----------|--------|-----------------|
| Code Quality | 🔴 / 🟡 / 🟢 | |
| Testing | 🔴 / 🟡 / 🟢 | |
| Security | 🔴 / 🟡 / 🟢 | |
| Documentation | 🔴 / 🟡 / 🟢 | |
| Observability | 🔴 / 🟡 / 🟢 | |
| Deployment | 🔴 / 🟡 / 🟢 | |

**Legend**: 🔴 Blocked | 🟡 In Progress | 🟢 Complete

---

## Code Quality

### Code Review

- [ ] All code has been reviewed by at least one other engineer
- [ ] Review comments have been addressed
- [ ] No unresolved conversations in PRs

### Code Standards

- [ ] Code follows [code-conventions.md](../../docs/dev/code-conventions.md)
- [ ] No linter warnings (`make lint` passes)
- [ ] No `TODO` or `FIXME` comments in shipped code (or tracked in Jira)
- [ ] Error handling follows project patterns
- [ ] Logging follows structured logging standards

### Technical Debt

- [ ] No known technical debt being shipped (or documented and tracked)
- [ ] No temporary workarounds without tickets

**Blocking Issues**:
- [ ] 

---

## Testing

### Unit Tests

- [ ] Unit tests written for all new code
- [ ] Coverage ≥ 80% on new code
- [ ] All unit tests pass (`make test`)
- [ ] Edge cases covered
- [ ] Error paths tested

### Integration Tests

- [ ] Integration tests for cross-service calls
- [ ] Database interactions tested
- [ ] External API interactions tested (or mocked appropriately)

### Manual Testing

- [ ] Happy path manually verified
- [ ] Error scenarios manually tested
- [ ] UI tested in browser (if applicable)

### Regression

- [ ] Existing tests still pass
- [ ] No regressions in related features

**Blocking Issues**:
- [ ] 

---

## Security

### Input Validation

- [ ] All external inputs validated
- [ ] No SQL injection vulnerabilities
- [ ] No command injection vulnerabilities
- [ ] XSS prevention (if UI)

### Authentication & Authorization

- [ ] Auth required for all endpoints
- [ ] Authorization checks in place
- [ ] Tenant isolation enforced

### Secrets

- [ ] No hardcoded secrets
- [ ] Secrets loaded from environment/K8s secrets
- [ ] No secrets in logs

### Security Review

- [ ] Security extension checklist passed
- [ ] `govulncheck` passes
- [ ] Dependency vulnerabilities addressed

**Blocking Issues**:
- [ ] 

---

## Documentation

### Code Documentation

- [ ] Public APIs documented
- [ ] Complex logic has comments
- [ ] README updated (if needed)

### User Documentation

- [ ] User-facing docs written (if applicable)
- [ ] Support docs updated
- [ ] Runbook created/updated

### API Documentation

- [ ] OpenAPI spec updated
- [ ] Proto files documented
- [ ] Breaking changes documented

**Blocking Issues**:
- [ ] 

---

## Observability

### Metrics

- [ ] Key metrics exposed
- [ ] Metrics follow naming conventions
- [ ] Cardinality is bounded

### Tracing

- [ ] Spans created for key operations
- [ ] Trace context propagated
- [ ] Span attributes include relevant context

### Logging

- [ ] Structured logging used
- [ ] Log levels appropriate
- [ ] No sensitive data in logs
- [ ] Correlation IDs included

### Dashboards & Alerts

- [ ] Grafana dashboard updated/created
- [ ] Key alerts configured
- [ ] On-call aware of new alerts

**Blocking Issues**:
- [ ] 

---

## Deployment

### Configuration

- [ ] Helm chart updated
- [ ] Environment variables documented
- [ ] Config changes reviewed

### Database

- [ ] Migrations tested
- [ ] Migrations are backwards compatible (or coordinated)
- [ ] Rollback migration exists

### Feature Flags

- [ ] Feature flag created in SplitIO
- [ ] Default state is OFF
- [ ] Targeting rules defined
- [ ] Kill switch tested

### Rollback Plan

- [ ] Rollback procedure documented
- [ ] Rollback tested (or clearly understood)
- [ ] Data rollback plan (if applicable)

**Blocking Issues**:
- [ ] 

---

## Sign-Off

| Role | Name | Date | Approved |
|------|------|------|----------|
| Tech Lead | | | ☐ |
| QA | | | ☐ |
| Security | | | ☐ |
| Product | | | ☐ |

---

## Post-Workshop Tasks

> List Jira tickets or work items needed to complete production readiness.

| Task | Priority | Assignee | Jira |
|------|----------|----------|------|
| | P1 / P2 / P3 | | |
| | | | |
| | | | |

---

## Notes

[Any additional context, risks, or considerations]
