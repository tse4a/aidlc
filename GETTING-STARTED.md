# Getting Started with AIDLC

A practical guide to using AIDLC for AI-assisted development.

---

## What You're Getting

This toolkit combines:

1. **AIDLC Methodology** — A structured workflow (Inception → Construction → Operations)
2. **Superpowers Plugin** — Tactical skills for brainstorming, planning, debugging
3. **Local Skills** — Security, testing, and pre-commit checklists

They work together: superpowers provides the execution, AIDLC provides the structure.

---

## Setup (One-Time)

### 1. Install Superpowers Plugin

```bash
claude plugins install superpowers
```

### 2. Copy AIDLC to Your Project

**Option A: Copy essential files**
```bash
# From the aidlc repo
cp AIDLC.md /path/to/your/project/
cp CLAUDE.md /path/to/your/project/
cp -r .claude /path/to/your/project/
mkdir -p /path/to/your/project/aidlc-docs
```

**Option B: Reference this repo**
Add to your project's CLAUDE.md:
```markdown
For complex features, follow AIDLC workflow from ~/Documents/aidlc/AIDLC.md
```

---

## Quick Reference: When to Use What

| Situation | What to Do |
|-----------|------------|
| Quick fix (< 1 hour) | Just do it. Think → Test → Code → Verify → Commit |
| New feature (complex) | Start with `/brainstorming` |
| Security-sensitive code | Use `/secure-code` |
| Before committing | Use `/pre-commit` |
| Writing tests | Use `/write-tests` |
| Debugging an issue | Use `/debugging` |
| Need implementation plan | Use `/writing-plans` |

---

## Workflow: Complex Feature

Here's how to build something non-trivial:

### Step 1: Brainstorm (Inception)

```
/brainstorming

I want to build [describe your feature]
```

The brainstorming skill will:
- Explore project context
- Ask clarifying questions (one at a time)
- Propose 2-3 approaches with trade-offs
- Present a design for approval
- Write a spec to `docs/superpowers/specs/`

**Don't skip this.** Even "simple" features benefit from 10 minutes of clarification.

### Step 2: Plan (Optional but Recommended)

After brainstorming approves your design:

```
/writing-plans
```

This creates a detailed implementation plan with:
- Ordered tasks
- File changes
- Test strategy

### Step 3: Build (Construction)

Work through the plan:

1. **Write tests first** — Use `/write-tests` for TDD guidance
2. **Implement** — Follow the plan
3. **Verify** — Run tests, linters

For security-sensitive code:
```
/secure-code
```

### Step 4: Commit (Quality Gate)

Before every commit:
```
/pre-commit
```

This runs:
- Tests
- Linters  
- Security scan
- Self-review checklist

### Step 5: Ship (Operations)

Use the production readiness checklist in `templates/operations/production-readiness.md`.

---

## Workflow: Quick Task

For small changes (< 1 hour), skip the formal process:

```
1. Think    — What am I building? Why?
2. Test     — Write a failing test
3. Code     — Make it pass
4. Verify   — Run test suite + lint
5. Commit   — Clear commit message
```

Use `/pre-commit` before committing even for quick tasks.

---

## Workflow: Debugging

When something's broken:

```
/debugging

[Describe the issue]
```

The debugging skill provides systematic troubleshooting.

---

## State Tracking (Optional)

For multi-session work, track progress in `aidlc-docs/state.md`:

```markdown
# State: [Feature Name]

## Current Phase
🟢 Construction

## Progress
- [x] Requirements analyzed
- [x] Design approved
- [ ] Component A complete
- [ ] Integration tested

## Next Step
Implement Component A
```

### Resuming Work

```
Check aidlc-docs/state.md and resume from where we left off.
```

---

## Skills Quick Reference

### Superpowers (Plugin)

| Skill | Purpose | When |
|-------|---------|------|
| `/brainstorming` | Clarify requirements, design | Starting complex work |
| `/writing-plans` | Detailed implementation plan | After design approved |
| `/debugging` | Systematic troubleshooting | When things break |

### Local Skills

| Skill | Purpose | When |
|-------|---------|------|
| `/secure-code` | Security hardening | Dockerfile, shell, API, database |
| `/pre-commit` | Quality checks | Before every commit |
| `/write-tests` | TDD workflow | Writing tests |

---

## Common Patterns

### "I have an idea but it's vague"

```
/brainstorming

I'm thinking about [vague idea]. Help me figure out what to build.
```

### "I need to add a feature to existing code"

```
/brainstorming

I need to add [feature] to [existing system]. 
The relevant code is in [path].
```

### "I'm about to write security-sensitive code"

```
/secure-code

I'm writing [Dockerfile / shell script / API endpoint / database query].
```

### "I want to follow TDD"

```
/write-tests

I need to implement [function/feature]. Help me write tests first.
```

### "Something's not working"

```
/debugging

[Error message or unexpected behavior]
```

---

## File Structure After Setup

```
your-project/
├── CLAUDE.md              # Project instructions (includes AIDLC reference)
├── AIDLC.md               # Full methodology (optional, can reference)
├── .claude/
│   ├── settings.json      # Enables superpowers
│   └── skills/            # Local skills
│       ├── secure-code/
│       ├── pre-commit/
│       └── write-tests/
├── aidlc-docs/            # Working documents (created as needed)
│   ├── state.md           # Progress tracking
│   ├── requirements.md    # Requirements + Q&A
│   └── design.md          # Technical design
├── docs/
│   └── superpowers/
│       └── specs/         # Specs from brainstorming
└── ... your code ...
```

---

## Tips

### Start Small
Don't use everything at once. Start with:
1. `/brainstorming` for new features
2. `/pre-commit` before commits

Add more as needed.

### Trust the Process
When `/brainstorming` asks clarifying questions, answer them. The upfront time saves rework later.

### Keep State Updated
If using `aidlc-docs/state.md`, update it when you stop. Future-you will thank present-you.

### Don't Over-Document
For quick tasks, skip the formal docs. The process should help, not slow you down.

---

## Troubleshooting

### "Superpowers isn't available"

```bash
# Check if installed
claude plugins list

# Install if missing
claude plugins install superpowers
```

### "Skills aren't loading"

Check that `.claude/settings.json` exists and has:
```json
{
  "enabledPlugins": {
    "superpowers@claude-plugins-official": true
  }
}
```

### "Too much process for my task"

Scale down:
- Quick task → Mental checklist only
- Standard task → Just `/brainstorming` + `/pre-commit`
- Complex task → Full workflow

---

## Next Steps

1. **Try it** — Pick a small feature and use `/brainstorming`
2. **Build the habit** — Always run `/pre-commit` before committing
3. **Expand** — Add more skills as your workflow evolves

Happy building!
