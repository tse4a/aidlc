# AIDLC Skills

Skills are on-demand guidance for common development workflows. They're designed to be invoked when needed rather than loaded upfront, keeping AI context efficient.

## Available Skills

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `secure-code` | Security hardening | Dockerfile, shell scripts, API endpoints, database queries |
| `pre-commit` | Quality checks before committing | Before any commit |
| `write-tests` | Test-driven development | Writing new tests, increasing coverage |

## How to Use

### With Claude Code / Copilot CLI

```
/secure-code
/pre-commit
/write-tests
```

### With Any AI Assistant

Reference the skill file in your prompt:
```
Read skills/secure-code.md and apply it to this code.
```

## Skill Structure

Each skill file includes:
- **When to use** — Trigger conditions
- **Workflow** — Step-by-step process
- **Checklists** — Quick reference items
- **Examples** — Concrete patterns

## Creating Custom Skills

1. Identify a repeatable workflow
2. Document the decision tree and checklists
3. Add concrete examples for each step
4. Save as `skills/<name>.md`

Skills should be:
- **Focused** — One workflow per skill
- **Actionable** — Clear steps, not just guidelines
- **Concrete** — Examples, not abstractions
