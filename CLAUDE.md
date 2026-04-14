# AIDLC Project

AI-Driven Development Life Cycle toolkit for structured, AI-assisted development.

## Quick Start

**New here?** Read [GETTING-STARTED.md](GETTING-STARTED.md) for a complete guide.

**Starting a feature:**
```
/brainstorming

I want to build [description]
```

**Resuming work:**
```
Check aidlc-docs/state.md and resume from where we left off.
```

---

## How Superpowers + AIDLC Work Together

**AIDLC** is the methodology (three phases, state tracking, decision records).
**Superpowers** provides tactical skills that execute within AIDLC phases.

```
┌─────────────────────────────────────────────────────────┐
│  AIDLC Phase          │  Superpowers Skill             │
├─────────────────────────────────────────────────────────┤
│  🔵 INCEPTION         │  /brainstorming                │
│     Understand &      │  - Clarify requirements        │
│     Design            │  - Ask questions               │
│                       │  - Design approach             │
│                       │  - Write spec                  │
├─────────────────────────────────────────────────────────┤
│  (Bridge)             │  /writing-plans                │
│                       │  - Detailed implementation     │
├─────────────────────────────────────────────────────────┤
│  🟢 CONSTRUCTION      │  /write-tests, /secure-code    │
│     Build & Test      │  - TDD workflow                │
│                       │  - Security hardening          │
│                       │  /pre-commit                   │
│                       │  - Quality gate                │
├─────────────────────────────────────────────────────────┤
│  🟡 OPERATIONS        │  (Use AIDLC templates)         │
│     Ship & Maintain   │  - Production readiness        │
└─────────────────────────────────────────────────────────┘
```

**In practice:** Use `/brainstorming` for inception, superpowers skills for construction, AIDLC templates for operations.

---

## Superpowers Plugin

Enabled in `.claude/settings.json`. Provides:

| Skill | Purpose | AIDLC Phase |
|-------|---------|-------------|
| `/brainstorming` | Clarify requirements, design approach | Inception |
| `/writing-plans` | Detailed implementation plan | Inception → Construction |
| `/debugging` | Systematic troubleshooting | Any |

---

## Local Skills

Project skills in `.claude/skills/` (token-efficient structure):

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `/secure-code` | Security hardening | Dockerfile, shell, API, database |
| `/pre-commit` | Quality checks | Before every commit |
| `/write-tests` | TDD workflow | Writing tests |

Each skill has:
- `SKILL.md` — Quick checklist (auto-loaded)
- `references/` — Deep-dive guides (loaded on demand)

---

## Workflow by Task Size

### Quick Task (< 1 hour)
```
Think → Test → Code → Verify → Commit
```
Use `/pre-commit` before committing.

### Standard Task (1h - 1 day)
```
/brainstorming → Build → /pre-commit → Commit
```

### Complex Task (> 1 day)
```
/brainstorming → /writing-plans → Build → /pre-commit → Operations checklist
```
Track state in `aidlc-docs/state.md`.

---

## State Tracking (Optional)

For multi-session work, use `aidlc-docs/state.md`:

```markdown
# State: [Feature Name]

## Current Phase
🟢 Construction

## Progress
- [x] Requirements (via /brainstorming)
- [x] Design approved
- [ ] Implementation
- [ ] Tests

## Next Step
[What to do next]
```

---

## References

- [GETTING-STARTED.md](GETTING-STARTED.md) — Complete user guide
- [AIDLC.md](AIDLC.md) — Full methodology documentation
- [docs/best-practices.md](docs/best-practices.md) — Development guidelines
- [docs/pre-commit-checklist.md](docs/pre-commit-checklist.md) — Quick reference
- [templates/](templates/) — Document templates
