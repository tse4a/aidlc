# Pre-Commit Checklist

Run through this checklist before every commit. Adapt commands for your tech stack.

---

## Quick Version

```bash
# 1. Tests
<your test command>           # All pass?

# 2. Lint
<your lint command>           # Clean?

# 3. Security (if available)
<your security scan>          # No vulnerabilities?

# 4. Commit
git add <files>
git commit -m "type: description"
```

---

## Full Checklist

### Code Quality

- [ ] **Tests pass** — All unit and integration tests
- [ ] **Linter clean** — No warnings or errors
- [ ] **No debug code** — Remove console.log, print statements, TODO comments
- [ ] **No commented code** — Delete or implement, don't leave commented

### Security

- [ ] **No hardcoded secrets** — API keys, passwords, tokens
- [ ] **Input validation** — All external inputs validated
- [ ] **No SQL injection** — Parameterized queries only
- [ ] **Safe error messages** — Don't leak internals

### Functionality

- [ ] **Feature works** — Manually tested happy path
- [ ] **Edge cases handled** — Error states, empty inputs
- [ ] **No regressions** — Existing functionality still works

### Documentation

- [ ] **Code is clear** — Comments where needed (why, not what)
- [ ] **README updated** — If adding features or changing setup
- [ ] **API docs updated** — If changing endpoints

---

## By Language

### Go

```bash
# Tests
go test ./...

# Lint
golangci-lint run

# Security
govulncheck ./...

# Format
gofmt -w .
```

### JavaScript/TypeScript

```bash
# Tests
npm test

# Lint
npm run lint

# Security
npm audit

# Format
npm run format
```

### Python

```bash
# Tests
pytest

# Lint
ruff check .
# or
flake8

# Security
bandit -r .

# Format
black .
```

### Rust

```bash
# Tests
cargo test

# Lint
cargo clippy

# Security
cargo audit

# Format
cargo fmt
```

---

## Git Commit Message Format

```
type: short description (max 72 chars)

Optional longer description explaining:
- Why this change is needed
- What approach was taken
- Any caveats or limitations

Fixes #123
```

### Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that neither fixes nor adds |
| `test` | Adding or updating tests |
| `chore` | Maintenance (deps, config, etc.) |

### Examples

```
feat: add user authentication endpoint

Implements JWT-based authentication with refresh tokens.
Tokens expire after 24 hours, refresh tokens after 7 days.

Closes #45
```

```
fix: handle empty response from API

The external API occasionally returns empty responses.
Now we retry once before failing.

Fixes #123
```

---

## When to Skip Checks

**Never skip security checks.**

You may skip other checks only when:

- Fixing a production incident (document why, fix properly later)
- Documentation-only changes (still run lint)
- Generated code (configure linter to ignore)

Always come back and fix properly.

---

## Automating Checks

### Git Hooks (pre-commit)

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

### CI Pipeline

Ensure your CI runs:

1. Build
2. Unit tests
3. Integration tests
4. Linting
5. Security scanning

Block merges if any fail.

---

## Quick Commands Reference

Copy and customize for your project:

```bash
# Add to your shell config or project scripts

alias precommit='
  echo "=== Tests ===" && <test> && \
  echo "=== Lint ===" && <lint> && \
  echo "=== Security ===" && <security> && \
  echo "=== All checks passed! ==="
'
```
