# Mid-Workflow Changes and Stage Management

## Overview

Users may request changes to the execution plan during the workshop. This document provides guidance on handling these requests safely.

---

## Types of Changes

### 1. Adding a Skipped Stage

**Scenario**: User wants to add a stage that was originally skipped.

**Example**: "Actually, I want to add user stories even though we skipped that."

**Handling**:

1. **Confirm**: "You want to add User Stories. This will take ~30 min. Confirm?"
2. **Check prerequisites**: Ensure required prior stages are complete
3. **Update state**: Add stage to `aidlc-state.md` as "PENDING"
4. **Execute**: Follow normal stage process
5. **Log**: Document in `audit.md` with timestamp and reason

---

### 2. Skipping a Planned Stage

**Scenario**: User wants to skip a planned stage.

**Example**: "Let's skip NFR Design for now — we're short on time."

**Handling**:

1. **Warn about impact**: "Skipping NFR Design means no caching/resilience patterns will be incorporated. Later stages may need manual setup."
2. **Get explicit confirmation**: User must acknowledge the impact
3. **Update state**: Mark stage as "SKIPPED: [reason]" in `aidlc-state.md`
4. **Log**: Document in `audit.md`

**Note**: Some stages cannot be skipped (Code Generation, Build and Test).

---

### 3. Restarting Current Stage

**Scenario**: User is unhappy with current stage results.

**Example**: "I don't like these requirements. Can we start over?"

**Handling**:

1. **Understand concern**: "What specifically would you like to change?"
2. **Offer options**:
   - **Modify**: Update existing artifacts (faster)
   - **Restart**: Clean slate (more thorough)
3. **If restart chosen**:
   - Archive existing: rename to `{file}.backup.md`
   - Reset checkboxes in `aidlc-state.md`
   - Re-execute from beginning
4. **Log**: Document reason for restart

---

### 4. Restarting a Previous Stage

**Scenario**: User wants to change a decision from earlier.

**Example**: "I want to change the architectural approach we decided earlier."

**Handling**:

1. **Assess impact**: List ALL stages that depend on this one
2. **Warn user**: "Restarting Application Design will require redoing: [list stages]. This adds ~2 hours."
3. **Get confirmation**: User must explicitly accept the impact
4. **If confirmed**:
   - Archive all affected artifacts
   - Reset all affected stages in `aidlc-state.md`
   - Return to the restart point
5. **Log**: Document full impact chain

**Consider**: Is modification possible instead of restart?

---

### 5. Changing Analysis Depth

**Scenario**: User wants more or less thorough analysis.

**Example**: "Let's do comprehensive requirements instead of standard."

**Handling**:

1. **Confirm**: "Changing to Comprehensive depth will add ~30 min but provide better coverage. Confirm?"
2. **Update approach**: Follow the new depth guidelines
3. **Adjust timeline**: Inform team of new estimates
4. **Log**: Document depth change

**Note**: Can only change depth before or during a stage, not after completion.

---

### 6. Pausing the Workshop

**Scenario**: Team needs to stop and resume later.

**Example**: "We need to break for lunch / stop for today."

**Handling**:

1. **Complete current step**: Finish the in-progress item if possible
2. **Update state**: Mark checkboxes for completed work
3. **Commit**: `git add workshop/ && git commit -m "AIDLC: paused at [stage]"`
4. **Log pause**: Document in `audit.md`
5. **Provide resume instructions**:
   ```
   To resume:
   Read workshop/AIDLC-WORKSHOP.md, then go to workshop/aidlc-docs/aidlc-state.md,
   find the first unchecked item, and resume from that point.
   ```

---

### 7. Adding/Removing Units

**Scenario**: Team wants to change the unit breakdown.

**Example**: "We need to split the Device unit into Device and Certificate."

**Handling**:

1. **Assess impact**: Which units have completed design/code?
2. **Explain consequences**:
   - Adding: Full design + code needed for new unit
   - Removing: Redistribute functionality
   - Splitting: Redo both resulting units
3. **Update artifacts**: Modify unit documentation
4. **Reset affected stages**: Mark as needing redo
5. **Execute**: Process affected units normally

---

### 8. Cross-Team Conflict

**Scenario**: Two teams made conflicting changes.

**Example**: "Team A and Team B both modified the proto file."

**Handling**:

1. **Stop both teams**: Do not merge yet
2. **Generate report**: List all conflicts
3. **Resolve together**: Bring teams to Cross-Team Alignment
4. **Decide**: Pick one approach, document rationale
5. **One team implements**: Other team reviews
6. **Log**: Document resolution in `audit.md`

---

## Decision Tree

```
User requests change
    │
    ├─ Current stage?
    │   ├─ Yes → Modify or restart current stage
    │   └─ No ↓
    │
    ├─ Completed stage?
    │   ├─ Yes → Assess cascade impact
    │   │   ├─ Low impact → Modify, update dependents
    │   │   └─ High impact → Restart from that stage
    │   └─ No ↓
    │
    ├─ Adding skipped stage?
    │   ├─ Yes → Check prereqs, add, execute
    │   └─ No ↓
    │
    ├─ Skipping planned stage?
    │   ├─ Yes → Warn impact, confirm, skip
    │   └─ No ↓
    │
    └─ Changing depth?
        ├─ Yes → Update plan, adjust approach
        └─ No → Clarify request
```

---

## Logging Requirements

Every change must be logged in `audit.md`:

```markdown
## Workflow Change
**Timestamp**: 2026-04-21T14:30:00Z
**Request**: [What user wanted to change]
**Current State**: [Stage and step we were at]
**Impact**: [What will be affected]
**User Confirmation**: [Explicit approval]
**Action Taken**: [What was done]
**Artifacts Affected**: [List of files]

---
```

---

## Best Practices

| Do | Don't |
|----|-------|
| ✅ Always confirm before changes | ❌ Make changes without explicit approval |
| ✅ Explain full impact | ❌ Downplay consequences |
| ✅ Archive before deleting | ❌ Delete without backup |
| ✅ Update ALL tracking files | ❌ Leave state inconsistent |
| ✅ Log everything | ❌ Skip audit entries |
| ✅ Offer alternatives | ❌ Force one approach |

---

## Workshop Time Management

When time is tight and changes are requested:

| If Time Left | Recommendation |
|--------------|----------------|
| > 2 hours | Full restart if needed |
| 1-2 hours | Prefer modification over restart |
| < 1 hour | Document for post-workshop, proceed with current |
| < 30 min | Focus on completing current stage only |

**Golden rule**: It's better to complete fewer features well than to leave many half-done.
