# Go Testing Patterns

Comprehensive guide to testing in Go.

## Basic Test Structure

```go
package mypackage_test  // External test package

import (
    "testing"
    
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
    
    "myproject/mypackage"
)

func TestFunction_Scenario(t *testing.T) {
    // Arrange
    input := "test"
    
    // Act
    result, err := mypackage.Function(input)
    
    // Assert
    require.NoError(t, err)
    assert.Equal(t, "expected", result)
}
```

## Table-Driven Tests

```go
func TestParseVersion(t *testing.T) {
    tests := []struct {
        name    string
        input   string
        want    Version
        wantErr bool
    }{
        {
            name:  "valid semver",
            input: "1.2.3",
            want:  Version{Major: 1, Minor: 2, Patch: 3},
        },
        {
            name:  "valid with v prefix",
            input: "v1.2.3",
            want:  Version{Major: 1, Minor: 2, Patch: 3},
        },
        {
            name:    "invalid format",
            input:   "not-a-version",
            wantErr: true,
        },
        {
            name:    "empty string",
            input:   "",
            wantErr: true,
        },
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := ParseVersion(tt.input)
            
            if tt.wantErr {
                require.Error(t, err)
                return
            }
            
            require.NoError(t, err)
            assert.Equal(t, tt.want, got)
        })
    }
}
```

## Mocking with Interfaces

```go
// Define interface for dependency
type UserRepository interface {
    Get(ctx context.Context, id string) (*User, error)
    Create(ctx context.Context, user *User) error
}

// Production implementation
type PostgresUserRepo struct {
    db *sql.DB
}

// Mock implementation for tests
type MockUserRepo struct {
    mock.Mock
}

func (m *MockUserRepo) Get(ctx context.Context, id string) (*User, error) {
    args := m.Called(ctx, id)
    if args.Get(0) == nil {
        return nil, args.Error(1)
    }
    return args.Get(0).(*User), args.Error(1)
}

func (m *MockUserRepo) Create(ctx context.Context, user *User) error {
    args := m.Called(ctx, user)
    return args.Error(0)
}

// Test using mock
func TestUserService_GetUser(t *testing.T) {
    // Arrange
    mockRepo := new(MockUserRepo)
    mockRepo.On("Get", mock.Anything, "user-123").Return(
        &User{ID: "user-123", Name: "Alice"},
        nil,
    )
    
    svc := NewUserService(mockRepo)
    
    // Act
    user, err := svc.GetUser(context.Background(), "user-123")
    
    // Assert
    require.NoError(t, err)
    assert.Equal(t, "Alice", user.Name)
    mockRepo.AssertExpectations(t)
}
```

## Testing HTTP Handlers

```go
func TestGetUserHandler(t *testing.T) {
    // Arrange
    mockSvc := new(MockUserService)
    mockSvc.On("GetUser", mock.Anything, "123").Return(
        &User{ID: "123", Name: "Alice"},
        nil,
    )
    
    handler := NewHandler(mockSvc)
    
    req := httptest.NewRequest("GET", "/users/123", nil)
    rec := httptest.NewRecorder()
    
    // Act
    handler.ServeHTTP(rec, req)
    
    // Assert
    assert.Equal(t, http.StatusOK, rec.Code)
    
    var resp User
    err := json.NewDecoder(rec.Body).Decode(&resp)
    require.NoError(t, err)
    assert.Equal(t, "Alice", resp.Name)
}
```

## Testing gRPC Services

```go
func TestUserService_CreateUser(t *testing.T) {
    // Arrange
    mockRepo := new(MockUserRepo)
    mockRepo.On("Create", mock.Anything, mock.AnythingOfType("*User")).Return(nil)
    
    svc := NewUserServiceServer(mockRepo)
    
    req := &pb.CreateUserRequest{
        Name:  "Alice",
        Email: "alice@example.com",
    }
    
    // Act
    resp, err := svc.CreateUser(context.Background(), req)
    
    // Assert
    require.NoError(t, err)
    assert.NotEmpty(t, resp.Id)
    assert.Equal(t, "Alice", resp.Name)
    mockRepo.AssertExpectations(t)
}
```

## Testing with Context

```go
func TestServiceWithTimeout(t *testing.T) {
    // Create context with timeout
    ctx, cancel := context.WithTimeout(context.Background(), 100*time.Millisecond)
    defer cancel()
    
    // Mock slow dependency
    mockRepo := new(MockRepo)
    mockRepo.On("SlowOperation", mock.Anything).Run(func(args mock.Arguments) {
        time.Sleep(200 * time.Millisecond)
    }).Return(nil, context.DeadlineExceeded)
    
    svc := NewService(mockRepo)
    
    // Act
    _, err := svc.DoSomething(ctx)
    
    // Assert
    assert.ErrorIs(t, err, context.DeadlineExceeded)
}
```

## Parallel Tests

```go
func TestParallel(t *testing.T) {
    tests := []struct {
        name  string
        input int
        want  int
    }{
        {"case1", 1, 2},
        {"case2", 2, 4},
        {"case3", 3, 6},
    }
    
    for _, tt := range tests {
        tt := tt  // Capture range variable
        t.Run(tt.name, func(t *testing.T) {
            t.Parallel()  // Run in parallel
            
            got := Double(tt.input)
            assert.Equal(t, tt.want, got)
        })
    }
}
```

## Test Helpers

```go
// Helper function
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()  // Marks as helper for better error reporting
    
    db, err := sql.Open("postgres", testDSN)
    require.NoError(t, err)
    
    t.Cleanup(func() {
        db.Close()
    })
    
    return db
}

func TestWithDB(t *testing.T) {
    db := setupTestDB(t)
    // Use db...
}
```

## Golden Files

```go
func TestOutputFormat(t *testing.T) {
    got := FormatOutput(input)
    
    golden := filepath.Join("testdata", t.Name()+".golden")
    
    if *update {
        os.WriteFile(golden, []byte(got), 0644)
    }
    
    want, err := os.ReadFile(golden)
    require.NoError(t, err)
    
    assert.Equal(t, string(want), got)
}
```

## Coverage

```bash
# Run with coverage
go test -cover ./...

# Generate coverage report
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out

# Check specific package coverage
go test -cover ./internal/auth/...
```

## Commands Reference

```bash
# Run all tests
go test ./...

# Verbose output
go test -v ./...

# Run specific test
go test -run TestCreateUser ./...

# Run with race detector
go test -race ./...

# Short mode (skip long tests)
go test -short ./...

# Set timeout
go test -timeout 30s ./...
```
