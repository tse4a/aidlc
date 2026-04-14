# Development Best Practices

Best practices integrated from production systems. Apply these to any project.

---

## Code Quality

### General Principles

- Keep functions focused and single-purpose
- Use meaningful variable names (avoid single-letter names except in short scopes)
- Prefer explicit over implicit
- Make invalid states unrepresentable

### Error Handling

- Always check and handle errors explicitly
- Wrap errors with context: `fmt.Errorf("operation failed: %w", err)`
- Return errors up the stack, handle at appropriate level
- Don't swallow errors silently

### Testing

- Write tests alongside code (TDD when practical)
- Aim for 80% coverage on new code
- Test edge cases and error paths
- Avoid flaky tests (no `time.Sleep()`, use channels/conditions)

---

## Security Rules

**Always enforce these — no exceptions:**

| Rule | Reason | Implementation |
|------|--------|----------------|
| Input validation | Prevent injection attacks | Validate all external inputs before use |
| Parameterized queries | Prevent SQL injection | Never concatenate strings into SQL |
| Secrets from environment | Prevent credential leaks | Use env vars or secret managers |
| No hardcoded credentials | Security | Never commit secrets to code |
| Error message safety | Information disclosure | Don't leak internal details to users |
| Timeout contexts | Resource exhaustion | All external calls need timeouts |
| Non-root containers | Privilege escalation | Run as non-root user |
| TLS everywhere | Data protection | Encrypt all network traffic |

### Security Checklist (Pre-Commit)

```markdown
- [ ] All external inputs validated
- [ ] No SQL string concatenation
- [ ] No hardcoded secrets
- [ ] Error messages don't leak internals
- [ ] External calls have timeout contexts
- [ ] Container runs as non-root
```

---

## Observability

### The Three Pillars

| Pillar | Purpose | Implementation |
|--------|---------|----------------|
| **Logs** | Debug information | Structured logging with context |
| **Metrics** | System health | Counters, gauges, histograms |
| **Traces** | Request flow | Distributed tracing across services |

### Logging Best Practices

- Use structured logging (JSON or key-value)
- Include correlation IDs (trace_id, request_id)
- Include relevant context (user_id, resource_id)
- Use appropriate log levels:
  - `ERROR`: Something failed, needs attention
  - `WARN`: Unexpected but handled
  - `INFO`: Important business events
  - `DEBUG`: Detailed debugging info

### What NOT to Log

- Passwords, tokens, API keys
- Personal identifiable information (PII)
- Credit card numbers
- Full request/response bodies (may contain sensitive data)

### Metrics Naming

- Use lowercase with underscores: `http_requests_total`
- Include units in name: `request_duration_seconds`
- Use standard suffixes: `_total`, `_count`, `_seconds`, `_bytes`

---

## API Design

### REST APIs

- Use resource-oriented URLs: `/users/{id}`, not `/getUser`
- Use HTTP methods correctly: GET (read), POST (create), PUT (update), DELETE
- Return appropriate status codes
- Version your API: `/v1/users`

### gRPC APIs

- Keep messages focused
- Use proper field types (not everything is a string)
- Include field validation in proto comments
- Use streaming for large data sets

### Error Responses

```json
{
  "error": {
    "code": "INVALID_INPUT",
    "message": "Email address is invalid",
    "details": {
      "field": "email",
      "value": "not-an-email"
    }
  }
}
```

Don't include:
- Stack traces
- Internal error messages
- Database details
- File paths

---

## Database Best Practices

### Queries

- Always use parameterized queries
- Add appropriate indexes
- Use pagination for large result sets
- Set query timeouts

### Migrations

- Make migrations reversible (up and down)
- Test migrations on copy of production data
- Never delete columns immediately (deprecate first)
- Use transactions for multi-step changes

### Connection Management

- Use connection pooling
- Set max connections appropriately
- Handle connection failures gracefully
- Close connections when done

---

## Git Workflow

### Commit Messages

```
type: short description

Longer explanation if needed.
- Bullet points for details
- Reference issues: Fixes #123

Co-Authored-By: Name <email>
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`

### Branch Strategy

- `main` — production-ready code
- `feat/description` — new features
- `fix/description` — bug fixes
- `chore/description` — maintenance

### Pre-Commit Checklist

```bash
# Run these before every commit:
<test command>        # All tests pass
<lint command>        # No lint errors
<security scan>       # No vulnerabilities
```

---

## Code Review Guidelines

### As Author

- Keep PRs small and focused
- Write clear PR description
- Self-review before requesting review
- Respond to feedback constructively

### As Reviewer

- Review within 24 hours
- Be specific in feedback
- Suggest, don't demand
- Approve when "good enough" (not perfect)

### What to Look For

- [ ] Does it work?
- [ ] Is it tested?
- [ ] Is it secure?
- [ ] Is it maintainable?
- [ ] Does it follow conventions?

---

## Performance Guidelines

### General

- Measure before optimizing
- Profile to find actual bottlenecks
- Cache expensive operations
- Use connection pooling

### What to Cache

- Database query results (with TTL)
- External API responses
- Computed values that don't change often

### What NOT to Cache

- User-specific sensitive data (carefully)
- Rapidly changing data
- Data that must be fresh

### Cache Invalidation

- Use TTL as safety net
- Invalidate on write
- Consider eventual consistency implications

---

## Documentation

### What to Document

- **README**: How to run the project
- **API docs**: How to use the API
- **Architecture**: High-level system design
- **Code comments**: Why, not what

### What NOT to Document

- Obvious code behavior
- Temporary workarounds (fix them instead)
- Outdated information (delete or update)

### Keep Docs Updated

- Update docs when you change code
- Review docs in code review
- Delete docs that are no longer accurate

---

## Dependency Management

### Adding Dependencies

- Prefer well-maintained libraries
- Check license compatibility
- Pin versions for reproducibility
- Audit for security vulnerabilities

### Updating Dependencies

- Update regularly (monthly)
- Read changelogs for breaking changes
- Run full test suite after updates
- Update in separate commits

---

## Quick Reference

### Before Starting a Feature

1. Understand the requirements
2. Check for existing patterns
3. Plan the approach
4. Write tests first (TDD)

### Before Committing

1. Tests pass
2. Linter clean
3. Security scan clean
4. Docs updated
5. Self-review complete

### Before Deploying

1. All checks pass
2. Feature flagged if needed
3. Monitoring in place
4. Rollback plan ready
