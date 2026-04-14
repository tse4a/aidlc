# Dockerfile Security Hardening

Comprehensive guide to securing Dockerfiles.

## Core Principles

1. **Minimize attack surface** — Install only what's needed
2. **Run as non-root** — Never run as root in production
3. **Pin versions** — Reproducible builds, known vulnerabilities
4. **No secrets in images** — Use runtime injection

---

## Version Pinning

### Base Images

```dockerfile
# ❌ BAD: Unpredictable, changes without notice
FROM golang:latest

# ⚠️ BETTER: Specific version
FROM golang:1.22-alpine

# ✅ BEST: Version + SHA256 digest
FROM golang:1.22-alpine@sha256:abc123...
```

### System Packages

```dockerfile
# ❌ BAD: Version can change
RUN apk add curl

# ✅ GOOD: Pinned version
RUN apk add curl=8.5.0-r0
```

---

## Non-Root User

```dockerfile
# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set ownership
COPY --chown=appuser:appgroup ./app /app

# Switch to non-root
USER appuser

# Verify
RUN whoami  # Should output: appuser
```

### Distroless Alternative

```dockerfile
FROM gcr.io/distroless/static-debian12:nonroot
USER nonroot:nonroot
```

---

## Multi-Stage Builds

```dockerfile
# Build stage - has tools
FROM golang:1.22-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app/server ./cmd/server

# Runtime stage - minimal
FROM gcr.io/distroless/static-debian12:nonroot
COPY --from=builder /app/server /server
USER nonroot:nonroot
ENTRYPOINT ["/server"]
```

Benefits:
- Build tools not in final image
- Smaller attack surface
- Smaller image size

---

## Variable Quoting

```dockerfile
# ❌ BAD: Word splitting, glob expansion
RUN echo $VERSION
COPY $SOURCE /app

# ✅ GOOD: Quoted variables
RUN echo "$VERSION"
COPY "$SOURCE" /app

# ✅ GOOD: Command substitution
RUN VERSION="$(cat version.txt)" && echo "$VERSION"
```

---

## Input Validation

When extracting values from files:

```dockerfile
# Extract version with validation
RUN VERSION="$(grep '^go ' go.mod | awk '{print $2}')" && \
    # Validate format (X.Y.Z)
    if ! echo "$VERSION" | grep -qE '^[0-9]+\.[0-9]+(\.[0-9]+)?$'; then \
        echo "Invalid version format: $VERSION" && exit 1; \
    fi && \
    echo "Using Go $VERSION"
```

---

## Secrets Management

```dockerfile
# ❌ NEVER: Secrets in image
ENV API_KEY=secret123
ARG PASSWORD=hunter2

# ✅ CORRECT: Runtime injection
# In docker-compose.yml or K8s:
# environment:
#   - API_KEY=${API_KEY}

# Or use Docker secrets (Swarm/Compose)
# Or use K8s secrets mounted as files
```

### Build-Time Secrets (BuildKit)

```dockerfile
# syntax=docker/dockerfile:1.4
RUN --mount=type=secret,id=npmrc,target=/root/.npmrc \
    npm install
```

---

## .dockerignore

Always include:

```
# Git
.git
.gitignore

# IDE
.vscode
.idea

# Secrets (CRITICAL)
.env
*.pem
*.key
credentials*

# Build artifacts
node_modules
vendor
dist
build

# Documentation
*.md
docs/
```

---

## Security Scanning

```bash
# Scan for vulnerabilities
docker scan myimage:latest

# Or use Trivy
trivy image myimage:latest

# Or use Grype
grype myimage:latest
```

---

## Complete Example

```dockerfile
# syntax=docker/dockerfile:1.4
FROM golang:1.22-alpine@sha256:abc123... AS builder

# Security: non-root for build
RUN adduser -D -u 1000 builder
USER builder

WORKDIR /app

# Dependencies first (cache layer)
COPY --chown=builder:builder go.mod go.sum ./
RUN go mod download && go mod verify

# Source code
COPY --chown=builder:builder . .

# Build with security flags
RUN CGO_ENABLED=0 GOOS=linux go build \
    -ldflags="-w -s" \
    -o /app/server ./cmd/server

# Runtime: minimal image
FROM gcr.io/distroless/static-debian12:nonroot

COPY --from=builder /app/server /server

USER nonroot:nonroot

EXPOSE 8080

ENTRYPOINT ["/server"]
```

---

## Checklist

Before committing any Dockerfile:

- [ ] Base image has specific version + SHA256
- [ ] System packages have pinned versions
- [ ] Final stage runs as non-root
- [ ] Multi-stage build (no build tools in runtime)
- [ ] All variables quoted
- [ ] No secrets in ENV/ARG
- [ ] .dockerignore excludes sensitive files
- [ ] Scanned for vulnerabilities
