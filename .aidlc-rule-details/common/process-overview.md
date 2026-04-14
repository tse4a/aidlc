# AI-DLC Adaptive Workflow Overview

**Purpose**: Technical reference for AI model to understand workflow structure.

---

## The Three-Phase Lifecycle

- **INCEPTION PHASE**: Planning and architecture (WHAT and WHY)
- **CONSTRUCTION PHASE**: Implementation and testing (HOW)
- **OPERATIONS PHASE**: Deployment (placeholder for future)

---

## Workshop-Adapted Flow

For Barracuda NetSec workshops with 2-3 teams on brownfield or greenfield projects:

**Brownfield** (adding to existing code): Includes Reverse Engineering stage
**Greenfield** (new service): Skips Reverse Engineering, more time on Application Design

```
Day 1: INCEPTION
━━━━━━━━━━━━━━━━
┌─────────────────────────────────────────────┐
│ Workspace Detection (all teams together)   │
│ • Scan existing codebase                    │
│ • Identify components, APIs, structure     │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Reverse Engineering (all teams together)   │
│ • Document existing architecture           │
│ • Map component ownership                  │
│ • Identify integration points              │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Requirements Analysis (per team)           │
│ • Load team's Vision document              │
│ • Clarify requirements                     │
│ • Gate: Team approves requirements         │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Cross-Team Alignment (all teams together)  │ ◀── Workshop-specific
│ • Share requirements                        │
│ • Agree on interface contracts             │
│ • Document shared touchpoints              │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Workflow Planning (per team)               │
│ • Plan Construction stages                 │
│ • Gate: Team approves plan                 │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Application Design (per team, if needed)   │
│ • Design new components                    │
│ • Gate: Team approves design               │
└─────────────────────────────────────────────┘


Day 2: CONSTRUCTION
━━━━━━━━━━━━━━━━━━━
┌─────────────────────────────────────────────┐
│ Functional Design (per unit, if needed)    │
│ • Detail business logic                    │
│ • Gate: Team approves                      │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Code Generation (ALWAYS)                   │
│ Part 1: Create plan with checkboxes        │
│ Part 2: Generate code + tests              │
│ • Gate: Team approves code                 │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Build and Test (ALWAYS)                    │
│ • make build                               │
│ • make test (must pass!)                   │
│ • make lint                                │
└─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────┐
│ Integration (all teams together)           │
│ • Wire up cross-team interfaces            │
│ • Test integration points                  │
│ • Resolve conflicts                        │
└─────────────────────────────────────────────┘
```

---

## Stage Execution Rules

### Always Execute
- Workspace Detection
- Requirements Analysis (adaptive depth)
- Workflow Planning
- Code Generation (with tests!)
- Build and Test

### Conditional
- Reverse Engineering (brownfield only — yes for workshop)
- Application Design (new components needed)
- Functional Design (complex business logic)

### Workshop-Specific
- Cross-Team Alignment (after Requirements, before Planning)
- Integration (after individual team code gen)

---

## Approval Gates

Every major stage requires explicit approval before proceeding:

```markdown
# [Stage] Complete

[Summary of what was produced]

> **REVIEW REQUIRED:**
> Please examine: `path/to/artifact.md`

> **WHAT'S NEXT?**
> - 🔧 Request Changes
> - ✅ Approve & Continue
```

Do NOT proceed until the team explicitly approves.

---

## State Tracking

Progress is tracked in `workshop/aidlc-docs/aidlc-state.md`:

```markdown
## Stage Progress

### 🔵 INCEPTION PHASE
- [x] Workspace Detection
- [x] Reverse Engineering
- [x] Requirements Analysis
- [x] Cross-Team Alignment
- [ ] Workflow Planning
- [ ] Application Design

### 🟢 CONSTRUCTION PHASE
- [ ] Functional Design
- [ ] Code Generation
- [ ] Build and Test
- [ ] Integration
```

Update checkboxes immediately after completing each stage.

---

## Audit Trail

All decisions logged in `workshop/aidlc-docs/audit.md`:

```markdown
## [Stage Name]
**Timestamp**: 2026-04-21T10:30:00Z
**User Input**: "[Exact user input]"
**AI Response**: "[What was done]"
**Context**: [Stage, decision made]

---
```

Never summarize user input — log it exactly as provided.

---

## Context Management

Clear context at every approval gate to prevent drift:

1. Complete stage
2. Get approval
3. Commit changes: `git add workshop/ && git commit -m "AIDLC: [stage] complete"`
4. Clear context (new chat or `/clear`)
5. Resume: "Go to workshop/aidlc-docs/aidlc-state.md and resume from first unchecked item"

---

## File Locations

| Content Type | Location |
|--------------|----------|
| AIDLC documentation | `workshop/aidlc-docs/` |
| Team inputs | `workshop/inputs/` |
| Generated code | Workspace root (NOT workshop/) |
| Tests | Alongside code in workspace |

**CRITICAL**: Application code goes in workspace root, never in `workshop/aidlc-docs/`.

---

## Related Rule Files

Load these as needed during workshop execution:

| File | When to Load |
|------|--------------|
| `welcome-message.md` | At session start (once) |
| `question-format-guide.md` | Before any requirements/clarification questions |
| `content-validation.md` | Before creating diagrams or documentation |
| `depth-levels.md` | At Workspace Detection to determine analysis depth |
| `error-handling.md` | When encountering failures or issues |
| `session-continuity.md` | When resuming after a break |
| `terminology.md` | When clarifying AIDLC terms |
| `overconfidence-prevention.md` | When generating questions (review periodically) |
| `workflow-changes.md` | When user requests mid-workflow modifications |
