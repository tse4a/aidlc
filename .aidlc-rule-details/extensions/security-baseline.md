# Security Baseline Extension

**Status**: Always enforced — No opt-in required

This extension enforces security best practices for all generated code.

---

## Mandatory Security Rules

All generated code must comply with these rules. Violations block stage progression.

### Input Validation

- [ ] All external inputs validated before use
- [ ] Type checking on API parameters
- [ ] Length/size limits enforced
- [ ] Reject unexpected fields in requests

### SQL/Database Security

- [ ] Parameterized queries only — NO string concatenation
- [ ] Prepared statements for all database operations
- [ ] Least privilege database connections

### Secrets Management

- [ ] Secrets from environment variables only
- [ ] No hardcoded credentials, API keys, or tokens
- [ ] Secrets not logged or included in error messages

### Error Handling

- [ ] Error messages don't leak internal details
- [ ] Stack traces not exposed to clients
- [ ] Sensitive data not included in logs

### Authentication & Authorization

- [ ] All endpoints require authentication (unless explicitly public)
- [ ] Authorization checks before accessing resources
- [ ] Session/token validation on every request

### Container Security

- [ ] Non-root container execution
- [ ] Minimal base images
- [ ] No unnecessary packages installed

### Network Security

- [ ] TLS for all external connections
- [ ] Certificate validation enabled
- [ ] Timeout contexts for all external calls

---

## Verification

Before approving Code Generation:

1. Review generated code against each rule
2. Flag any violations
3. Regenerate with corrections if needed
4. Do NOT proceed with violations

---

## Adding Project-Specific Rules

Create additional extension files in `.aidlc-rule-details/extensions/` for:

- Compliance requirements (HIPAA, SOC2, PCI-DSS)
- Organization-specific policies
- Project-specific constraints

Extension files with `.opt-in.md` suffix require explicit user opt-in.
Extension files without `.opt-in.md` are always enforced.
