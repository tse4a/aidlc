# Error Handling

## Recovery Procedures for AIDLC Workshop Failures

---

## Missing or Corrupted Artifacts

### Symptom
Required files from previous stages are missing or unreadable.

### Recovery

1. **Check git history**:
   ```bash
   git log --oneline workshop/aidlc-docs/
   ```

2. **Restore from last commit**:
   ```bash
   git checkout HEAD~1 -- workshop/aidlc-docs/<path>
   ```

3. **If no git history**, restart from last known good state:
   - Check `aidlc-state.md` for last completed stage
   - Re-run the stage that produced missing artifact

4. **Document the recovery** in `audit.md`:
   ```markdown
   ## Recovery Event
   **Timestamp**: [ISO timestamp]
   **Issue**: Missing artifact: [filename]
   **Resolution**: Restored from commit [hash] / Re-ran [stage]
   ```

---

## Stage Execution Failures

### Build Failure

1. Read the full error output
2. Identify the failing component
3. Check if it's a code issue or environment issue
4. **Do NOT regenerate code blindly** — update the design first if needed
5. Fix and re-run build

### Test Failure

1. Read which tests failed and why
2. Categorize:
   - **Test bug**: Fix the test
   - **Code bug**: Update design, then fix code
   - **Missing test data**: Add fixtures
3. Re-run: `make test`

### Lint Failure

1. Run `make lint-fix` for auto-fixable issues
2. For manual fixes, address each linter error
3. Re-run: `make lint`

---

## Context Confusion

### Symptom
AI references wrong files, forgets prior decisions, or makes contradictory statements.

### Recovery

1. **Clear context**: `/clear` or start new chat
2. **Resume properly**:
   ```
   Read workshop/AIDLC-WORKSHOP.md, then go to workshop/aidlc-docs/aidlc-state.md,
   find the first unchecked item, and resume from that point.
   ```
3. **If confusion persists**, explicitly load required artifacts:
   ```
   Please read these files before continuing:
   - workshop/aidlc-docs/inception/requirements/<team>-requirements.md
   - workshop/aidlc-docs/inception/application-design/components.md
   ```

---

## Cross-Team Conflicts

### Symptom
Two teams modified the same file or have incompatible changes.

### Recovery

1. **Stop both teams** — do not merge yet
2. **Generate conflict report**:
   ```
   Review all files modified by any team today. Report files touched by
   multiple teams and describe the conflicts. Do not edit — produce a report.
   ```
3. **Resolve together** in Cross-Team Alignment
4. **Document decision** in `audit.md`
5. **One team makes the change**, other team reviews

---

## Session Interruption

### Symptom
Workshop session interrupted (network, time, emergency).

### Recovery

1. **Commit current state** (if possible):
   ```bash
   git add workshop/ && git commit -m "AIDLC Workshop: WIP - interrupted at [stage]"
   ```

2. **When resuming**, use continuity prompt:
   ```
   Read workshop/AIDLC-WORKSHOP.md, then go to workshop/aidlc-docs/aidlc-state.md,
   find the first unchecked item, and resume from that point.
   ```

3. **Verify state** by checking:
   - Last entry in `audit.md`
   - Checkboxes in `aidlc-state.md`
   - Git log for last commit

---

## Question File Issues

### Missing [Answer]: Tags

```
The question file is missing [Answer]: tags. Please add them after each question
in the format shown in question-format-guide.md.
```

### Partial Answers

```
Questions [X, Y, Z] are not answered. Please complete all questions before proceeding.
```

### Invalid Format

```
Answers must be letter choices (A, B, C, etc.). Please update your responses
to use the letter format.
```

---

## Extension Violations

### Symptom
Generated code violates security or compliance rules.

### Recovery

1. **Identify the violation** (which rule, which code)
2. **Do NOT just fix the code** — update the design first
3. **Regenerate** with explicit instruction:
   ```
   This code violates rule [X] from secureedge-security.md.
   Please update the functional design to address [specific issue],
   then regenerate the code.
   ```
4. **Verify** the regenerated code passes the rule check

---

## Escalation

If recovery procedures don't work:

1. **Document the issue** in `audit.md` with full details
2. **Tag for facilitator help** in your team's status
3. **Continue with other work** if possible (different unit/stage)
4. **Do NOT make up solutions** — wait for guidance

---

## Prevention Checklist

| Action | Prevents |
|--------|----------|
| Commit at every gate | Lost work |
| Clear context after approvals | Context confusion |
| Load artifacts before resuming | Missing context |
| Use question files (not chat) | Lost answers |
| Check for contradictions | Bad requirements |
| Run build/test after each unit | Late failures |
