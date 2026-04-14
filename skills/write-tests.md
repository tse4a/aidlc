# Write Tests Skill

Test-driven development workflow. Write tests first, then implement.

---

## When to Use

- Adding new functionality
- Fixing bugs (regression test first)
- Increasing test coverage
- Refactoring existing code

---

## TDD Workflow

```
┌─────────────────────────────────────┐
│ 1. RED: Write Failing Test          │
│    Test describes desired behavior  │
└───────────────┬─────────────────────┘
                │ Test fails?
                ▼
┌─────────────────────────────────────┐
│ 2. GREEN: Make It Pass              │
│    Minimum code to pass test        │
└───────────────┬─────────────────────┘
                │ Test passes?
                ▼
┌─────────────────────────────────────┐
│ 3. REFACTOR: Clean Up               │
│    Improve code, tests still pass   │
└───────────────┬─────────────────────┘
                │ Tests pass?
                ▼
             Repeat
```

---

## Step 1: RED - Write Failing Test

### What to Test

1. **Happy path** — Normal expected usage
2. **Edge cases** — Boundaries, empty inputs, limits
3. **Error cases** — Invalid inputs, failures

### Test Structure (Arrange-Act-Assert)

```go
func TestCreateUser_Success(t *testing.T) {
    // Arrange: Set up test data and dependencies
    svc := NewService(mockDB)
    req := &CreateUserRequest{
        Name:  "Alice",
        Email: "alice@example.com",
    }
    
    // Act: Call the function
    user, err := svc.CreateUser(ctx, req)
    
    // Assert: Verify results
    require.NoError(t, err)
    assert.Equal(t, "Alice", user.Name)
    assert.Equal(t, "alice@example.com", user.Email)
}
```

### Naming Convention

```
Test<Function>_<Scenario>

TestCreateUser_Success
TestCreateUser_EmptyEmail_ReturnsError
TestCreateUser_DuplicateEmail_ReturnsConflict
```

### Run and Verify It Fails

```bash
go test ./... -run TestCreateUser
```

The test MUST fail before you write implementation.

---

## Step 2: GREEN - Make It Pass

### Rules

1. Write minimum code to pass the test
2. Don't over-engineer
3. Don't add untested functionality

### Example

```go
// Minimum implementation to pass test
func (s *Service) CreateUser(ctx context.Context, req *CreateUserRequest) (*User, error) {
    user := &User{
        Name:  req.Name,
        Email: req.Email,
    }
    return user, nil
}
```

### Run and Verify It Passes

```bash
go test ./... -run TestCreateUser
```

---

## Step 3: REFACTOR - Clean Up

### What to Refactor

- Remove duplication
- Improve naming
- Extract helper functions
- Simplify logic

### Rules

- Tests must still pass after refactoring
- Don't change behavior
- Run tests after each small change

---

## Test Patterns

### Table-Driven Tests

For testing multiple inputs/outputs:

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
        {"no_domain", "user@", true},
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

### Testing Error Cases

```go
func TestGetUser_NotFound_ReturnsError(t *testing.T) {
    svc := NewService(mockDB)
    mockDB.On("GetUser", "nonexistent").Return(nil, sql.ErrNoRows)
    
    _, err := svc.GetUser(ctx, "nonexistent")
    
    assert.ErrorIs(t, err, ErrUserNotFound)
}
```

### Testing with Mocks

```go
// Interface for dependency
type UserRepository interface {
    Get(ctx context.Context, id string) (*User, error)
    Create(ctx context.Context, user *User) error
}

// Mock in tests
type mockRepo struct {
    mock.Mock
}

func (m *mockRepo) Get(ctx context.Context, id string) (*User, error) {
    args := m.Called(ctx, id)
    if args.Get(0) == nil {
        return nil, args.Error(1)
    }
    return args.Get(0).(*User), args.Error(1)
}
```

---

## Coverage

### Check Coverage

```bash
# Go
go test ./... -cover
go test ./... -coverprofile=coverage.out
go tool cover -html=coverage.out

# JavaScript
npm test -- --coverage

# Python
pytest --cov=.
```

### Coverage Targets

- **New code**: 80% minimum
- **Critical paths**: 100%
- **Happy path + error handling**: Always

### What NOT to Cover

- Generated code
- Simple getters/setters
- Code that just delegates

---

## Regression Tests

When fixing a bug:

1. **Write test that reproduces the bug**
2. **Verify test fails** (proves bug exists)
3. **Fix the bug**
4. **Verify test passes** (proves fix works)

```go
// This test will prevent the bug from returning
func TestProcessOrder_NegativeQuantity_ReturnsError(t *testing.T) {
    // This used to crash; now it returns an error
    svc := NewOrderService()
    
    _, err := svc.ProcessOrder(ctx, &Order{Quantity: -1})
    
    assert.Error(t, err)
    assert.Contains(t, err.Error(), "invalid quantity")
}
```

---

## Quick Reference

| What | How |
|------|-----|
| Test naming | `Test<Function>_<Scenario>` |
| Test structure | Arrange-Act-Assert |
| TDD cycle | Red → Green → Refactor |
| Coverage target | 80% new code |
| Regression test | Write failing test first |
| Multiple cases | Table-driven tests |
| Dependencies | Use interfaces + mocks |

---

## Anti-Patterns

### Don't Test Implementation Details

```go
// Bad: Tests internal state
assert.Equal(t, 3, len(svc.internalCache))

// Good: Tests behavior
result, _ := svc.Get("key")
assert.Equal(t, "value", result)
```

### Don't Skip Edge Cases

```go
// Bad: Only happy path
func TestParse(t *testing.T) {
    result := Parse("valid input")
    assert.NotNil(t, result)
}

// Good: Edge cases covered
func TestParse_EmptyInput_ReturnsError(t *testing.T) { ... }
func TestParse_InvalidFormat_ReturnsError(t *testing.T) { ... }
func TestParse_ValidInput_ReturnsResult(t *testing.T) { ... }
```

### Don't Use Flaky Tests

```go
// Bad: Depends on timing
time.Sleep(100 * time.Millisecond)
assert.True(t, done)

// Good: Use channels or conditions
select {
case <-done:
    // success
case <-time.After(1 * time.Second):
    t.Fatal("timeout waiting for completion")
}
```
