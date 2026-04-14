# API Security Hardening

Comprehensive guide to securing API endpoints.

## Core Principles

1. **Validate all inputs** — Never trust client data
2. **Authenticate and authorize** — Verify identity and permissions
3. **Use timeouts** — Prevent resource exhaustion
4. **Return safe errors** — Don't leak internals

---

## Input Validation

### Validate at the Boundary

```go
func (s *Server) CreateUser(ctx context.Context, req *CreateUserRequest) (*User, error) {
    // Validate FIRST, before any processing
    if err := validateCreateUserRequest(req); err != nil {
        return nil, status.Error(codes.InvalidArgument, err.Error())
    }
    
    // Now safe to process
    return s.userService.Create(ctx, req)
}

func validateCreateUserRequest(req *CreateUserRequest) error {
    if req == nil {
        return errors.New("request required")
    }
    
    if req.Email == "" {
        return errors.New("email required")
    }
    
    if !isValidEmail(req.Email) {
        return errors.New("invalid email format")
    }
    
    if len(req.Name) > 100 {
        return errors.New("name too long")
    }
    
    return nil
}
```

### Common Validations

| Field Type | Validation |
|------------|------------|
| Email | Regex + length limit |
| UUID | Format check |
| String | Length limits, allowed characters |
| Number | Range check, type |
| URL | Scheme whitelist, format |
| File path | No traversal, allowed directories |

---

## Authentication

### Token Validation

```go
func (s *Server) authenticate(ctx context.Context) (*User, error) {
    // Extract token
    md, ok := metadata.FromIncomingContext(ctx)
    if !ok {
        return nil, status.Error(codes.Unauthenticated, "missing metadata")
    }
    
    tokens := md.Get("authorization")
    if len(tokens) == 0 {
        return nil, status.Error(codes.Unauthenticated, "missing token")
    }
    
    token := strings.TrimPrefix(tokens[0], "Bearer ")
    
    // Validate token
    user, err := s.tokenService.Validate(ctx, token)
    if err != nil {
        // Log actual error, return generic message
        log.Error("token validation failed", "error", err)
        return nil, status.Error(codes.Unauthenticated, "invalid token")
    }
    
    return user, nil
}
```

---

## Authorization

### Check Permissions

```go
func (s *Server) DeleteResource(ctx context.Context, req *DeleteRequest) (*Empty, error) {
    // Authenticate
    user, err := s.authenticate(ctx)
    if err != nil {
        return nil, err
    }
    
    // Get resource
    resource, err := s.resourceService.Get(ctx, req.Id)
    if err != nil {
        return nil, status.Error(codes.NotFound, "resource not found")
    }
    
    // Authorize: Check ownership or admin
    if resource.OwnerID != user.ID && !user.IsAdmin {
        // Log attempt, return generic error
        log.Warn("unauthorized delete attempt",
            "user_id", user.ID,
            "resource_id", req.Id)
        return nil, status.Error(codes.PermissionDenied, "not authorized")
    }
    
    // Proceed with deletion
    return s.resourceService.Delete(ctx, req.Id)
}
```

---

## Timeout Contexts

### Always Set Timeouts

```go
func (s *Server) ProcessRequest(ctx context.Context, req *Request) (*Response, error) {
    // Create timeout context
    ctx, cancel := context.WithTimeout(ctx, 30*time.Second)
    defer cancel()
    
    // All downstream calls inherit timeout
    result, err := s.externalService.Call(ctx, req)
    if err != nil {
        if ctx.Err() == context.DeadlineExceeded {
            return nil, status.Error(codes.DeadlineExceeded, "request timeout")
        }
        return nil, status.Error(codes.Internal, "operation failed")
    }
    
    return result, nil
}
```

### Timeout Guidelines

| Operation | Suggested Timeout |
|-----------|------------------|
| Database query | 5-10 seconds |
| External API call | 10-30 seconds |
| File upload | 60+ seconds |
| Health check | 2-5 seconds |

---

## Safe Error Responses

### Don't Leak Internals

```go
// ❌ BAD: Leaks internal details
func (s *Server) GetUser(ctx context.Context, req *GetUserRequest) (*User, error) {
    user, err := s.db.Query("SELECT * FROM users WHERE id = $1", req.Id)
    if err != nil {
        // Exposes database error and query
        return nil, fmt.Errorf("database error: %v", err)
    }
    return user, nil
}

// ✅ GOOD: Safe error response
func (s *Server) GetUser(ctx context.Context, req *GetUserRequest) (*User, error) {
    user, err := s.db.Query("SELECT * FROM users WHERE id = $1", req.Id)
    if err != nil {
        // Log full details server-side
        log.Error("failed to get user",
            "error", err,
            "user_id", req.Id)
        
        // Return generic message to client
        if errors.Is(err, sql.ErrNoRows) {
            return nil, status.Error(codes.NotFound, "user not found")
        }
        return nil, status.Error(codes.Internal, "operation failed")
    }
    return user, nil
}
```

### Error Response Format

```json
{
    "error": {
        "code": "INVALID_INPUT",
        "message": "Email address is invalid"
    }
}
```

**Never include:**
- Stack traces
- SQL queries
- File paths
- Internal error messages
- System information

---

## Rate Limiting

```go
// Simple in-memory rate limiter
type RateLimiter struct {
    requests map[string][]time.Time
    limit    int
    window   time.Duration
    mu       sync.Mutex
}

func (r *RateLimiter) Allow(key string) bool {
    r.mu.Lock()
    defer r.mu.Unlock()
    
    now := time.Now()
    cutoff := now.Add(-r.window)
    
    // Clean old requests
    valid := []time.Time{}
    for _, t := range r.requests[key] {
        if t.After(cutoff) {
            valid = append(valid, t)
        }
    }
    
    if len(valid) >= r.limit {
        return false
    }
    
    r.requests[key] = append(valid, now)
    return true
}
```

---

## Complete Example

```go
func (s *Server) UpdateProfile(ctx context.Context, req *UpdateProfileRequest) (*Profile, error) {
    // 1. Timeout
    ctx, cancel := context.WithTimeout(ctx, 10*time.Second)
    defer cancel()
    
    // 2. Input validation
    if err := validateUpdateProfileRequest(req); err != nil {
        return nil, status.Error(codes.InvalidArgument, err.Error())
    }
    
    // 3. Authentication
    user, err := s.authenticate(ctx)
    if err != nil {
        return nil, err
    }
    
    // 4. Authorization
    if req.UserId != user.ID && !user.IsAdmin {
        log.Warn("unauthorized profile update",
            "requester", user.ID,
            "target", req.UserId)
        return nil, status.Error(codes.PermissionDenied, "not authorized")
    }
    
    // 5. Rate limiting
    if !s.rateLimiter.Allow(user.ID) {
        return nil, status.Error(codes.ResourceExhausted, "rate limit exceeded")
    }
    
    // 6. Business logic
    profile, err := s.profileService.Update(ctx, req)
    if err != nil {
        log.Error("profile update failed",
            "error", err,
            "user_id", req.UserId)
        return nil, status.Error(codes.Internal, "update failed")
    }
    
    return profile, nil
}
```

---

## Checklist

Before deploying any API endpoint:

- [ ] All inputs validated
- [ ] Authentication required (or explicitly public)
- [ ] Authorization checked for protected resources
- [ ] Timeout context set
- [ ] Error responses don't leak internals
- [ ] Rate limiting in place
- [ ] Request/response logged (without sensitive data)
- [ ] HTTPS only
