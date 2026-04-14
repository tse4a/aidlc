# AIDLC Workshop Welcome Message

**Purpose**: Display this message ONCE at the start of any AIDLC workshop session.

---

## Welcome Message Template

```markdown
# Welcome to the AIDLC Workshop!

I'll guide you through a structured development workflow adapted from AWS AI-DLC
for Barracuda NetSec engineering.

## What is AIDLC?

AIDLC (AI-Driven Development Life Cycle) is a structured yet adaptive process that:

- **Analyzes requirements** and asks clarifying questions
- **Plans the approach** based on complexity and risk
- **Adapts depth** — thorough for complex work, streamlined for simple changes
- **Documents decisions** with complete audit trail
- **Enforces quality gates** with checkpoints and approvals

## The Three-Phase Lifecycle

```
Day 1: INCEPTION PHASE
━━━━━━━━━━━━━━━━━━━━━━
┌─────────────────────────────────────────┐
│ Planning & Application Design           │
│ • Workspace Detection (ALWAYS)          │
│ • Reverse Engineering (BROWNFIELD)      │
│ • Requirements Analysis (ALWAYS)        │
│ • Cross-Team Alignment (WORKSHOP)       │
│ • Workflow Planning (ALWAYS)            │
│ • Application Design (CONDITIONAL)      │
└─────────────────────────────────────────┘

Day 2: CONSTRUCTION PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━
┌─────────────────────────────────────────┐
│ Design, Implementation & Test           │
│ • Functional Design (CONDITIONAL)       │
│ • Code Generation (ALWAYS)              │
│ • Build and Test (ALWAYS)               │
│ • Integration (WORKSHOP)                │
└─────────────────────────────────────────┘

End of Day 2: OPERATIONS PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
┌─────────────────────────────────────────┐
│ Production Readiness                    │
│ • Production Readiness Assessment       │
│ • Deployment Planning                   │
│ • Post-Workshop Tasks                   │
└─────────────────────────────────────────┘
```

## Your Role

- **Answer questions** in dedicated question files using [Answer]: tags
- **Review and approve** each stage before proceeding
- **Collaborate** with your team on decisions
- **Stay in your lane** — each team edits only their owned files

## Key Rules

1. **Tests are mandatory** — generate tests alongside code
2. **Commit at gates** — `git commit` after each approval
3. **Clear context** — start fresh after each stage
4. **No vibe coding** — update design docs before changing code
5. **Questions in files** — use [Answer]: tags, not chat responses

## What Happens Next

1. **Workspace Detection** — I'll scan the codebase
2. **Load your Vision** — from `workshop/inputs/<team>-vision.md`
3. **Requirements Analysis** — clarifying questions in a file
4. **You approve** — then we proceed to the next stage

Ready to begin?
```

---

## When to Show

Display this message when:

1. User starts a new AIDLC session with no existing `aidlc-state.md`
2. User explicitly requests AIDLC workflow start

Do NOT show when:

1. Resuming an existing session (use session-continuity prompts instead)
2. User is in the middle of a stage

---

## Post-Welcome Actions

After displaying the welcome message:

1. Create `workshop/aidlc-docs/aidlc-state.md` if it doesn't exist
2. Log the session start in `workshop/aidlc-docs/audit.md`
3. Proceed to Workspace Detection stage
