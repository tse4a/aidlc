# AIDLC Solo Mode

> **For single-person projects** — Streamlined AIDLC workflow when you're working alone.

---

## When to Use Solo Mode

- Personal projects
- Side projects
- Learning/experimentation
- Small features (< 1 day of work)
- Prototypes

For team workshops, use the full `AIDLC-WORKSHOP.md` instead.

---

## Solo Workflow Overview

```
┌─────────────────────────────────────────┐
│     🔵 QUICK INCEPTION (30-60 min)      │
├─────────────────────────────────────────┤
│ • Workspace Detection (5 min)           │
│ • Requirements Sketch (15-30 min)       │
│ • Design Sketch (15-30 min)             │
└─────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────┐
│     🟢 BUILD (varies)                   │
├─────────────────────────────────────────┤
│ • Code + Tests (iterative)              │
│ • Build & Verify                        │
└─────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────┐
│     🟡 WRAP UP (15 min)                 │
├─────────────────────────────────────────┤
│ • Quick checklist                       │
│ • Document what's left                  │
└─────────────────────────────────────────┘
```

---

## Phase 1: Quick Inception (30-60 min)

### Step 1: Workspace Detection (5 min)

Ask the AI:
```
Read AIDLC-WORKSHOP.md. This is a solo project. 
Scan the workspace and tell me what you see.
```

### Step 2: Requirements Sketch (15-30 min)

Create `aidlc-docs/requirements.md`:

```markdown
# Requirements: [Feature Name]

## What I'm Building
[1-2 sentences]

## Why
[Motivation]

## Success Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

## Out of Scope
- [Thing I'm NOT doing]

## Open Questions
- [Question 1]?
```

Ask the AI to review and suggest clarifications. Answer them inline.

### Step 3: Design Sketch (15-30 min)

Create `aidlc-docs/design.md`:

```markdown
# Design: [Feature Name]

## Approach
[Brief description of how you'll build it]

## Key Components
- [Component 1]: [Purpose]
- [Component 2]: [Purpose]

## Data Flow
[How data moves through the system]

## Edge Cases
- [Edge case 1]: [How to handle]

## Testing Strategy
- Unit tests for: [X, Y, Z]
- Integration test for: [Critical path]
```

**Gate**: Review the design. Does it make sense? Then proceed.

---

## Phase 2: Build (varies)

### Iterative Development

Work in small increments:

1. **Pick one thing** from your success criteria
2. **Write a test** for it (if applicable)
3. **Implement** the feature
4. **Verify** it works
5. **Commit**: `git commit -m "Add [feature]"`
6. **Repeat**

### Code Generation with AI

If using AI to generate code:

```
Based on aidlc-docs/design.md, generate [specific component].
Include tests. Follow the patterns in templates/tech-env.md.
```

Review before accepting. Update design if you discover issues.

### Build & Verify

Run your build and test commands:

```bash
# Your build command
# Your test command
# Your lint command
```

All must pass before moving on.

---

## Phase 3: Wrap Up (15 min)

### Quick Checklist

```markdown
# Completion Check

## Code Quality
- [ ] All tests pass
- [ ] Linter clean
- [ ] No hardcoded secrets
- [ ] Error handling complete

## Documentation
- [ ] README updated (if needed)
- [ ] Comments for non-obvious code

## What's Left
- [ ] [Future improvement 1]
- [ ] [Future improvement 2]
```

### Final Commit

```bash
git add -A
git commit -m "Complete [feature name]"
```

---

## Solo Mode Tips

### Keep It Light

- Don't over-document
- Skip stages that don't add value
- Design sketch can be mental for tiny changes

### When to Escalate to Full AIDLC

- Feature is larger than expected
- Multiple components involved
- Need to integrate with other systems
- Security-sensitive functionality

### Minimum Viable Process

For very small changes (< 1 hour):

1. **Think** — What am I building?
2. **Test** — Write a failing test
3. **Code** — Make it pass
4. **Verify** — Run full test suite
5. **Commit** — Done

---

## Adapting the Templates

### For Solo Projects

| Full AIDLC | Solo Mode |
|------------|-----------|
| Vision Document | Mental model or 1-paragraph note |
| Tech Environment | Use existing or skip |
| Cross-Team Alignment | Skip |
| Production Readiness | Quick checklist |
| Facilitator Guide | Not needed |

### Question Format

For solo work, you can answer questions in chat instead of files.
But for complex decisions, writing them down helps clarify thinking.

---

## Example: Adding a Feature (Solo)

```markdown
# Session Log

## 1. What I'm Building
Add caching to the user lookup API to reduce database load.

## 2. Quick Design
- Add Redis cache layer
- Cache key: `user:{id}`
- TTL: 5 minutes
- Invalidate on user update

## 3. Implementation Plan
- [ ] Add Redis client
- [ ] Create cache wrapper
- [ ] Update GetUser to check cache first
- [ ] Add cache invalidation on UpdateUser
- [ ] Add tests

## 4. Progress
[x] Add Redis client - done
[x] Create cache wrapper - done
[ ] Update GetUser... (in progress)
```

This lightweight tracking keeps you focused without overhead.

---

## Starting a Solo Session

```
Read SOLO-MODE.md. I'm working on [brief description].
Help me through the solo AIDLC workflow.
```

The AI will guide you through the streamlined process.
