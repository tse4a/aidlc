# AIDLC — AI-Driven Development Life Cycle

> A structured workflow for AI-assisted software development. Provides the right
> amount of process: thorough for complex work, streamlined for simple changes.

---

## Quick Start

**Starting a new feature or project:**
```
Read AIDLC.md. Help me build [brief description].
```

**Resuming work:**
```
Read AIDLC.md, check aidlc-docs/state.md, and resume from where we left off.
```

---

## Core Principles

1. **Understand before building** — Clarify requirements, don't assume
2. **Design before coding** — Plan the approach, identify risks
3. **Test alongside code** — Never skip tests
4. **Document decisions** — Track why, not just what
5. **Validate at gates** — Check work before moving forward

---

## The Three Phases

```
┌─────────────────────────────────────┐
│     🔵 INCEPTION                    │
│     Understand & Design             │
├─────────────────────────────────────┤
│ • Analyze requirements              │
│ • Ask clarifying questions          │
│ • Design the approach               │
└─────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────┐
│     🟢 CONSTRUCTION                 │
│     Build & Test                    │
├─────────────────────────────────────┤
│ • Generate code with tests          │
│ • Build and verify                  │
│ • Iterate until complete            │
└─────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────┐
│     🟡 OPERATIONS                   │
│     Ship & Maintain                 │
├─────────────────────────────────────┤
│ • Production readiness check        │
│ • Deploy                            │
│ • Document what's left              │
└─────────────────────────────────────┘
```

---

## Scaling to Task Size

### Quick Tasks (< 1 hour)

Skip formal documents. Mental checklist:

1. **Think** — What am I building? Why?
2. **Test** — Write a failing test
3. **Code** — Make it pass
4. **Verify** — Run full test suite
5. **Commit** — Done

### Standard Tasks (1 hour - 1 day)

Lightweight documentation:

```markdown
# Feature: [Name]

## What
[1-2 sentences]

## Why
[Motivation]

## Approach
[How you'll build it]

## Done When
- [ ] [Criterion 1]
- [ ] [Criterion 2]
```

### Complex Tasks (> 1 day)

Full AIDLC workflow with formal documents. See detailed phases below.

---

## Phase 1: Inception

### Step 1: Understand the Request

Before writing any code, understand:
- What exactly needs to be built?
- Why is it needed?
- What are the constraints?
- What does success look like?

### Step 2: Analyze Existing Code (Brownfield)

For existing codebases:
1. Scan the workspace structure
2. Identify relevant components
3. Understand existing patterns
4. Document in `aidlc-docs/analysis.md`

### Step 3: Clarify Requirements

Create `aidlc-docs/requirements.md`:

```markdown
# Requirements: [Feature Name]

## Summary
[What we're building]

## Questions

### Q1: [Question]?
[Answer]: 

### Q2: [Question]?
[Answer]: 

## Success Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## Out of Scope
- [Thing we're NOT doing]
```

**Gate**: Requirements approved before proceeding.

### Step 4: Design the Approach

Create `aidlc-docs/design.md`:

```markdown
# Design: [Feature Name]

## Approach
[How we'll build it]

## Components
- [Component 1]: [Purpose]
- [Component 2]: [Purpose]

## Data Flow
[How data moves through the system]

## Edge Cases
- [Case 1]: [How to handle]

## Testing Strategy
- Unit tests for: [X, Y, Z]
- Integration test for: [Critical path]
```

**Gate**: Design approved before proceeding.

---

## Phase 2: Construction

### Step 1: Plan the Work

Break down into small, testable units:

```markdown
# Implementation Plan

- [ ] Create [component A]
- [ ] Add tests for [component A]
- [ ] Create [component B]
- [ ] Add tests for [component B]
- [ ] Wire up [A] and [B]
- [ ] Add integration test
```

### Step 2: Build Iteratively

For each unit:
1. Write a failing test (RED)
2. Write minimum code to pass (GREEN)
3. Refactor if needed
4. Commit

### Step 3: Verify

```bash
# Run all quality checks
<test command>           # All pass?
<lint command>           # Clean?
<security scan>          # No vulnerabilities?
```

**Gate**: All checks pass before proceeding.

---

## Phase 3: Operations

### Production Readiness Check

Before deploying, verify:

```markdown
## Code Quality
- [ ] All tests pass
- [ ] Linter clean
- [ ] No hardcoded secrets
- [ ] Error handling complete

## Observability
- [ ] Logging in place
- [ ] Metrics if needed
- [ ] Alerts if critical

## Documentation
- [ ] README updated
- [ ] API docs updated (if applicable)

## Deployment
- [ ] Config changes documented
- [ ] Rollback plan exists
- [ ] Feature flag if risky
```

### Document Remaining Work

```markdown
## What's Left
- [ ] [Future improvement 1]
- [ ] [Future improvement 2]
```

---

## State Tracking

For complex work, track progress in `aidlc-docs/state.md`:

```markdown
# State: [Feature Name]

## Current Phase
🔵 Inception / 🟢 Construction / 🟡 Operations

## Progress
- [x] Requirements analyzed
- [x] Design approved
- [ ] Component A complete
- [ ] Component B complete
- [ ] Integration tested

## Blockers
- [Any blockers]

## Next Step
[What to do next]
```

---

## Session Continuity

### Ending a Session

Before stopping:
1. Update `aidlc-docs/state.md` with current progress
2. Commit work in progress
3. Note the next step clearly

### Resuming a Session

```
Read AIDLC.md, check aidlc-docs/state.md, and resume from where we left off.
```

---

## Key Rules

### Questions in Files, Not Chat

Use `[Answer]:` tags in markdown files:

```markdown
### Q: Should we use caching?
[Answer]: Yes, Redis with 5-minute TTL for user lookups.
```

This creates a permanent record of decisions.

### No "Vibe Coding"

If you find an issue during implementation:
1. Go back to the design document
2. Update the design
3. Then update the code

Don't fix code without updating the design that produced it.

### Commit at Gates

After each approval gate:
```bash
git add aidlc-docs/
git commit -m "docs: complete [stage name]"
```

### Test Everything

Never skip tests. Generate tests alongside code, not after.

---

## Directory Structure

```
your-project/
├── AIDLC.md                   # This file (or reference it)
├── aidlc-docs/                # AIDLC working documents
│   ├── state.md               # Progress tracking
│   ├── requirements.md        # Requirements + Q&A
│   ├── design.md              # Technical design
│   └── decisions.md           # Decision log (optional)
└── ... your code ...
```

---

## Skills

On-demand guidance for common workflows:

| Skill | When to Use |
|-------|-------------|
| `secure-code` | Security-sensitive code |
| `pre-commit` | Before committing |
| `write-tests` | Writing tests (TDD) |

See `skills/` directory for details.

---

## References

- [AWS AI-DLC Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [AI-DLC GitHub](https://github.com/awslabs/aidlc-workflows)
- [Best Practices](docs/best-practices.md)
