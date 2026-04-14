# AIDLC Project

AI-Driven Development Life Cycle toolkit for structured, AI-assisted development.

## Quick Start

```
Read AIDLC.md. Help me build [description].
```

## Plugins

This project uses the **superpowers** plugin for enhanced workflows:

- `/brainstorming` — Turn ideas into designs before implementation
- `/writing-plans` — Create detailed implementation plans
- `/debugging` — Systematic debugging workflow

The plugin is enabled in `.claude/settings.json`.

## Skills

Local skills in `.claude/skills/`:

| Skill | Purpose | Invoke |
|-------|---------|--------|
| `secure-code` | Security hardening | `/secure-code` |
| `pre-commit` | Quality checks | `/pre-commit` |
| `write-tests` | TDD workflow | `/write-tests` |

Skills auto-trigger based on context. Each has:
- `SKILL.md` — Quick checklist (token-efficient)
- `references/` — Deep-dive guides (loaded on demand)

## Workflow

### For Complex Features

1. **Brainstorm first** — Use `/brainstorming` to clarify requirements and design
2. **Plan** — Use `/writing-plans` for implementation steps
3. **Build** — Follow TDD with `/write-tests`
4. **Verify** — Use `/pre-commit` before committing
5. **Secure** — Use `/secure-code` for security-sensitive code

### For Quick Tasks

Skip formal process. Mental checklist:
1. Think → 2. Test → 3. Code → 4. Verify → 5. Commit

## References

- [AIDLC.md](AIDLC.md) — Full workflow documentation
- [docs/best-practices.md](docs/best-practices.md) — Development guidelines
- [docs/pre-commit-checklist.md](docs/pre-commit-checklist.md) — Quick reference
