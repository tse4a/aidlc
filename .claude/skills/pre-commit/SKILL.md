---
name: pre-commit
description: Quality workflow before committing code. Use when user says "commit", "pre-commit", "ready to commit", "quality check", or before any git commit.
---

# Pre-Commit Skill

Quality checks to run before every commit. For language-specific details, see `references/` directory.

## Quick Workflow

```
1. Tests     → All pass?
2. Lint      → Clean?
3. Security  → No vulnerabilities?
4. Review    → Self-review diff
5. Commit    → Clear message
```

---

## Step 1: Tests

```bash
# Go
go test ./...

# JavaScript/TypeScript
npm test

# Python
pytest

# Rust
cargo test
```

**Must pass:** All tests green.

---

## Step 2: Lint

```bash
# Go
golangci-lint run

# JavaScript/TypeScript
npm run lint

# Python
ruff check .

# Rust
cargo clippy
```

**Must pass:** Zero warnings.

---

## Step 3: Security

```bash
# Go
govulncheck ./...

# JavaScript/TypeScript
npm audit

# Python
bandit -r .

# Rust
cargo audit
```

**Must pass:** No vulnerabilities.

---

## Step 4: Self-Review

```bash
git diff --staged
```

Check:
- [ ] No debug code (console.log, print, TODO)
- [ ] No commented-out code
- [ ] No hardcoded secrets
- [ ] Error handling complete

---

## Step 5: Commit

### Message Format

```
type: short description (max 72 chars)

Optional body explaining why.

Fixes #123
```

### Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation |
| `refactor` | Code change (no new feature/fix) |
| `test` | Tests |
| `chore` | Maintenance |

---

## Language-Specific Details

For comprehensive checklists by language, see:
- `references/go-checklist.md`
- `references/js-checklist.md`
- `references/python-checklist.md`
- `references/rust-checklist.md`

---

## Quick Reference

| Check | Command (Go) | Must Pass |
|-------|--------------|-----------|
| Tests | `go test ./...` | All green |
| Lint | `golangci-lint run` | Zero warnings |
| Security | `govulncheck ./...` | No vulns |
| Format | `gofmt -w .` | Auto-fixed |
