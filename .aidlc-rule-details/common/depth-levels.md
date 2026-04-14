# Depth Levels

## Adaptive Analysis Depth

AIDLC adapts its analysis depth based on request complexity. This prevents over-engineering simple changes and under-analyzing complex ones.

---

## Three Depth Levels

### Minimal

**When to use:**
- Bug fixes with clear root cause
- Configuration changes
- Documentation updates
- Single-file changes
- Copy/paste patterns from existing code

**Characteristics:**
- Skip most conditional stages
- Abbreviated requirements (1-2 questions)
- Direct to Code Generation
- Minimal design documentation

**Example requests:**
- "Fix the null pointer in GetDevice"
- "Update the timeout from 30s to 60s"
- "Add logging to the sync handler"

---

### Standard

**When to use:**
- New features within existing patterns
- Multi-file changes in one service
- API additions (not modifications)
- Test coverage improvements

**Characteristics:**
- Requirements Analysis with 3-5 questions
- Skip User Stories (unless user-facing)
- Functional Design if complex logic
- Standard Code Generation flow

**Example requests:**
- "Add a new RPC to get device posture"
- "Implement caching for user details"
- "Add integration tests for enrollment"

---

### Comprehensive

**When to use:**
- New services or major components
- Breaking API changes
- Cross-service features
- Architecture changes
- Security-sensitive features
- Performance-critical paths

**Characteristics:**
- Full Requirements Analysis (5+ questions)
- User Stories (if user-facing)
- Application Design
- Units Generation (if parallelizable)
- Full Construction loop per unit
- NFR Requirements + Design
- Infrastructure Design

**Example requests:**
- "Build a new conditional access service"
- "Redesign the certificate rotation flow"
- "Add multi-tenant support"

---

## Depth Selection Matrix

| Indicator | Minimal | Standard | Comprehensive |
|-----------|---------|----------|---------------|
| Files affected | 1-2 | 3-10 | 10+ |
| Services affected | 1 | 1 | 2+ |
| New APIs | 0 | 1-2 | 3+ |
| Breaking changes | No | No | Possibly |
| User-facing | No | Maybe | Yes |
| Security impact | Low | Medium | High |
| Performance critical | No | Maybe | Yes |
| New patterns introduced | No | No | Yes |

---

## Workshop Context

For workshop sessions, depth is often predetermined:

| Project Type | Typical Depth |
|--------------|---------------|
| Bug fix workshop | Minimal |
| Feature sprint | Standard |
| New service workshop | Comprehensive |
| Hackathon | Minimal to Standard |

**Override**: Teams can request different depth if justified.

---

## Depth Adjustment

### Escalate Depth When

- Hidden complexity discovered during analysis
- Cross-team dependencies identified
- Security concerns raised
- Performance requirements added

**Process:**
1. Document the reason in `audit.md`
2. Notify the team
3. Add newly-required stages to `aidlc-state.md`
4. Continue from current point

### Reduce Depth When

- Scope is cut during workshop
- Time constraints require focus
- Requirements simplified

**Process:**
1. Document the reason in `audit.md`
2. Get team approval
3. Mark skipped stages as "Skipped: [reason]" in `aidlc-state.md`
4. Continue with remaining stages

---

## Per-Stage Depth Adaptation

Even within a depth level, individual stages adapt:

### Requirements Analysis

| Depth | Questions | Detail Level |
|-------|-----------|--------------|
| Minimal | 1-2 | Confirm scope only |
| Standard | 3-5 | Clarify approach |
| Comprehensive | 5-10 | Full specification |

### Functional Design

| Depth | Content |
|-------|---------|
| Minimal | Skip or 1 paragraph |
| Standard | Data models + key logic |
| Comprehensive | Full business rules, edge cases, state machines |

### Code Generation

| Depth | Approach |
|-------|----------|
| Minimal | Generate directly, minimal plan |
| Standard | Plan with checkboxes, generate in order |
| Comprehensive | Detailed plan, unit-by-unit, verification at each step |

---

## Indicators for AI

When analyzing a request, check these signals:

**Minimal signals:**
- "fix", "update", "change", "tweak"
- Single file path mentioned
- Reference to existing pattern

**Standard signals:**
- "add", "implement", "create"
- Feature name without "service" or "system"
- Builds on existing infrastructure

**Comprehensive signals:**
- "build", "design", "architect", "redesign"
- "service", "system", "platform"
- Multiple teams mentioned
- Security/compliance requirements
- Performance SLAs mentioned
