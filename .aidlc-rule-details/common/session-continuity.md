# Session Continuity

## Resuming Work

When returning to an AIDLC workshop session, use this prompt:

```
Read workshop/AIDLC-WORKSHOP.md, then go to workshop/aidlc-docs/aidlc-state.md,
find the first unchecked item, and resume from that point.
```

## Context Loading by Stage

Before resuming any stage, load relevant artifacts:

| Resuming At | Load These Files |
|-------------|------------------|
| Reverse Engineering | `aidlc-state.md` |
| Requirements Analysis | `reverse-engineering/*.md` |
| Cross-Team Alignment | All team `requirements/*.md` |
| Workflow Planning | `requirements/*.md`, `cross-team-contracts.md` |
| Application Design | All inception artifacts |
| Functional Design | All inception + prior construction artifacts |
| Code Generation | All design artifacts |
| Build and Test | Generated code files |

## Welcome Back Prompt

When detecting existing workshop state, present:

```markdown
**Welcome back to the AIDLC Workshop!**

Based on aidlc-state.md:
- **Current Phase**: [INCEPTION/CONSTRUCTION]
- **Last Completed**: [Stage name]
- **Next Step**: [Stage name]

**What would you like to do?**
A) Continue where you left off
B) Review a previous stage

[Answer]:
```

## Cross-Team Session Handoff

When one team hands off to another:

```markdown
## Handoff: [From Team] → [To Team]
**Timestamp**: [ISO timestamp]
**Completed by [From Team]**:
- [List of completed stages/artifacts]

**Ready for [To Team]**:
- [What they can now work on]

**Shared Artifacts**:
- `cross-team-contracts.md` — Interface agreements
- `reverse-engineering/architecture.md` — System overview
```

## Error Recovery

If artifacts are missing or corrupted:

1. Check git history: `git log --oneline workshop/`
2. Restore from last commit: `git checkout HEAD~1 -- workshop/aidlc-docs/`
3. If no recovery possible, restart from last known good state

## Commit Points

Commit at these points to enable recovery:

- After Workspace Detection completes
- After Reverse Engineering completes
- After each team's Requirements Analysis
- After Cross-Team Alignment
- After Workflow Planning
- After Application Design
- After Code Generation (each unit)
- After Build and Test passes

Commit message format:
```
AIDLC Workshop: [Stage] complete

Team: [team name]
Artifacts: [list of files created/updated]
```
