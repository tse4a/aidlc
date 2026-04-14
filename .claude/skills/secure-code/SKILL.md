---
name: secure-code
description: Security hardening workflows for Dockerfiles, shell scripts, APIs, and databases. Use when user says "security review", "harden", "validate input", "SQL injection", or "Dockerfile security".
---

# Secure Code Skill

Fast security review based on what you're working on. For comprehensive guides, see `references/` directory.

## Quick Decision Tree

```
What are you securing?
├─ Dockerfile
│  └─ See: Quick Dockerfile Checklist (below)
│      Deep-dive: references/dockerfile-hardening.md
│
├─ Shell script
│  └─ See: Quick Shell Script Checklist (below)
│      Deep-dive: references/shell-hardening.md
│
├─ API endpoint
│  └─ See: Quick API Checklist (below)
│      Deep-dive: references/api-hardening.md
│
└─ Database query
   └─ See: Quick Database Checklist (below)
       Deep-dive: references/database-hardening.md
```

---

## Quick Dockerfile Checklist

- [ ] **Variables quoted:** `"$VAR"` not `$VAR`
- [ ] **Specific versions:** No `latest` tags
- [ ] **Non-root user:** `USER nonroot` in final stage
- [ ] **No secrets:** No hardcoded credentials in ENV/ARG
- [ ] **Multi-stage build:** Don't ship build tools

**Need details?** See `references/dockerfile-hardening.md`

---

## Quick Shell Script Checklist

- [ ] **Strict mode:** `set -euo pipefail`
- [ ] **Variables quoted:** `"$VAR"` not `$VAR`
- [ ] **Inputs validated:** Check before use
- [ ] **Secure temp files:** Use `mktemp`
- [ ] **Cleanup traps:** `trap 'cleanup' EXIT`

**Need details?** See `references/shell-hardening.md`

---

## Quick API Checklist

- [ ] **Input validation:** Validate all external inputs
- [ ] **Authentication:** Verify identity
- [ ] **Authorization:** Check permissions
- [ ] **Timeout context:** All external calls have timeouts
- [ ] **Safe errors:** Don't leak internals

**Need details?** See `references/api-hardening.md`

---

## Quick Database Checklist

- [ ] **Parameterized queries:** Never concatenate SQL
- [ ] **Least privilege:** Minimal permissions
- [ ] **Query timeouts:** Set context deadlines
- [ ] **Connection limits:** Use pooling

**Example violation:**
```go
// ❌ NEVER
query := "SELECT * FROM users WHERE id = '" + userInput + "'"

// ✅ ALWAYS
query := "SELECT * FROM users WHERE id = $1"
row := db.QueryRowContext(ctx, query, userInput)
```

**Need details?** See `references/database-hardening.md`

---

## Quick Reference

| Area | Key Rule |
|------|----------|
| SQL | Parameterized queries only |
| Secrets | Environment/secret manager only |
| Errors | Log details, return generic |
| Input | Validate at boundary |
| Docker | Non-root, pinned versions |
| Shell | Quote variables, set -euo pipefail |
| API | Timeout context, authz check |
