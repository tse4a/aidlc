---
name: write-tests
description: Test-driven development workflow. Use when user says "write tests", "add tests", "TDD", "test coverage", "unit test", or "integration test".
---

# Write Tests Skill

TDD workflow: Red → Green → Refactor. For language-specific patterns, see `references/` directory.

## TDD Cycle

```
┌─────────────────────────────────────┐
│ 1. RED: Write Failing Test          │
│    Test describes desired behavior  │
└───────────────┬─────────────────────┘
                │ Test fails?
                ▼
┌─────────────────────────────────────┐
│ 2. GREEN: Make It Pass              │
│    Minimum code to pass             │
└───────────────┬─────────────────────┘
                │ Test passes?
                ▼
┌─────────────────────────────────────┐
│ 3. REFACTOR: Clean Up               │
│    Improve code, tests still pass   │
└─────────────────────────────────────┘
```

---

## What to Test

1. **Happy path** — Normal expected usage
2. **Edge cases** — Boundaries, empty inputs
3. **Error cases** — Invalid inputs, failures

---

## Test Structure

**Arrange-Act-Assert:**

```go
func TestCreateUser_Success(t *testing.T) {
    // Arrange
    svc := NewService(mockDB)
    req := &CreateUserRequest{Name: "Alice"}
    
    // Act
    user, err := svc.CreateUser(ctx, req)
    
    // Assert
    require.NoError(t, err)
    assert.Equal(t, "Alice", user.Name)
}
```

---

## Naming Convention

```
Test<Function>_<Scenario>

TestCreateUser_Success
TestCreateUser_EmptyName_ReturnsError
TestCreateUser_Duplicate_ReturnsConflict
```

---

## Table-Driven Tests

For multiple cases:

```go
func TestValidateEmail(t *testing.T) {
    tests := []struct {
        name    string
        email   string
        wantErr bool
    }{
        {"valid", "user@example.com", false},
        {"empty", "", true},
        {"no_at", "userexample.com", true},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            err := ValidateEmail(tt.email)
            if tt.wantErr {
                assert.Error(t, err)
            } else {
                assert.NoError(t, err)
            }
        })
    }
}
```

---

## Regression Tests

When fixing a bug:

1. Write test that reproduces the bug
2. Verify test **fails**
3. Fix the bug
4. Verify test **passes**

---

## Coverage

```bash
# Go
go test -cover ./...

# JavaScript
npm test -- --coverage

# Python
pytest --cov=.
```

**Target:** 80% on new code, 100% on critical paths.

---

## Language-Specific Guides

- `references/go-testing.md` — Go patterns, mocks, subtests
- `references/js-testing.md` — Jest, React Testing Library
- `references/python-testing.md` — pytest, fixtures

---

## Quick Reference

| What | How |
|------|-----|
| Naming | `Test<Function>_<Scenario>` |
| Structure | Arrange-Act-Assert |
| TDD | Red → Green → Refactor |
| Coverage | 80% new code |
| Regression | Write failing test first |

---

## Anti-Patterns

- Testing implementation details (internal state)
- Only testing happy path
- Flaky tests (time.Sleep, random data)
- Mocking everything (test nothing)
