# Pre-Commit Skill

Quality workflow to run before every commit. Catches issues before they reach code review.

---

## When to Use

- Before every `git commit`
- Before creating a PR
- After significant changes

---

## Workflow

```
┌─────────────────────────────────────┐
│ 1. Run Tests                        │
│    All tests must pass              │
└───────────────┬─────────────────────┘
                │ Pass?
                ▼
┌─────────────────────────────────────┐
│ 2. Run Linters                      │
│    No warnings or errors            │
└───────────────┬─────────────────────┘
                │ Clean?
                ▼
┌─────────────────────────────────────┐
│ 3. Security Scan                    │
│    No vulnerabilities               │
└───────────────┬─────────────────────┘
                │ Clear?
                ▼
┌─────────────────────────────────────┐
│ 4. Review Changes                   │
│    Self-review diff                 │
└───────────────┬─────────────────────┘
                │ Good?
                ▼
┌─────────────────────────────────────┐
│ 5. Commit                           │
│    Clear, descriptive message       │
└─────────────────────────────────────┘
```

---

## Step 1: Run Tests

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

**Must pass:** All tests green, no skipped tests hiding failures.

---

## Step 2: Run Linters

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

**Must pass:** Zero warnings, zero errors.

---

## Step 3: Security Scan

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

**Must pass:** No known vulnerabilities in dependencies.

---

## Step 4: Review Changes

```bash
git diff --staged
```

### Self-Review Checklist

- [ ] No debug code (console.log, print, TODO)
- [ ] No commented-out code
- [ ] No hardcoded secrets
- [ ] No unintended file changes
- [ ] Error handling complete
- [ ] Tests cover new code

---

## Step 5: Commit

### Message Format

```
type: short description (max 72 chars)

Optional longer description:
- Why this change is needed
- What approach was taken
- Any limitations

Fixes #123
```

### Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation |
| `refactor` | Code change (no new feature/fix) |
| `test` | Adding/updating tests |
| `chore` | Maintenance |

### Examples

```
feat: add user authentication endpoint

Implements JWT-based auth with refresh tokens.
Access tokens expire after 24h, refresh after 7d.

Closes #45
```

```
fix: handle empty API response

The external API occasionally returns empty responses.
Now retries once before failing.

Fixes #123
```

---

## Quick Commands by Language

### Go

```bash
go test ./... && \
golangci-lint run && \
govulncheck ./... && \
git diff --staged
```

### JavaScript/TypeScript

```bash
npm test && \
npm run lint && \
npm audit && \
git diff --staged
```

### Python

```bash
pytest && \
ruff check . && \
bandit -r . && \
git diff --staged
```

### Rust

```bash
cargo test && \
cargo clippy && \
cargo audit && \
git diff --staged
```

---

## Troubleshooting

### Tests Fail

1. Read the failure message
2. Check if it's a real bug or test issue
3. Fix the root cause
4. Run tests again

### Linter Errors

1. Most can be auto-fixed: `golangci-lint run --fix`
2. For others, read the error and fix manually
3. Don't disable linter rules without good reason

### Security Vulnerabilities

1. Check if the vulnerability affects your usage
2. Update the dependency if possible
3. If can't update, document the risk and mitigation

### Pre-Commit Hook Setup

Create `.git/hooks/pre-commit`:

```bash
#!/bin/sh
set -e

echo "Running tests..."
<test command>

echo "Running linter..."
<lint command>

echo "All checks passed!"
```

Make executable: `chmod +x .git/hooks/pre-commit`

---

## When to Skip Checks

**Never skip security checks.**

You may skip other checks only when:
- Fixing a production incident (document why, fix properly later)
- Documentation-only changes (still run lint)

Always come back and fix properly.
