# AIDLC — AI-Driven Development Life Cycle

A practical toolkit for AI-assisted software development. Based on 
[AWS AI-DLC](https://github.com/awslabs/aidlc-workflows), streamlined for 
everyday use.

## What is AIDLC?

AIDLC is a structured workflow that helps you:

- **Clarify requirements** before writing code
- **Design approaches** based on complexity
- **Track decisions** with a clear audit trail
- **Maintain quality** through gates and checklists

It scales from quick fixes to complex features — use what you need.

## Quick Start

### Option 1: Copy to Your Project

```bash
# Copy the essentials
cp AIDLC.md /path/to/your/project/
mkdir -p /path/to/your/project/aidlc-docs
```

### Option 2: Reference This Repo

Add to your project's CLAUDE.md or instructions:
```
For complex features, follow the AIDLC workflow from [path/to/AIDLC.md]
```

### Starting a Session

```
Read AIDLC.md. Help me build [brief description].
```

## Directory Structure

```
aidlc/
├── AIDLC.md                   # Main workflow (start here)
├── skills/                    # On-demand development guidance
│   ├── secure-code.md         # Security hardening
│   ├── pre-commit.md          # Quality checks
│   └── write-tests.md         # TDD workflow
├── docs/                      # Reference materials
│   ├── best-practices.md      # Development guidelines
│   └── pre-commit-checklist.md
├── templates/                 # Document templates
│   ├── tech-env.md            # Technical environment
│   ├── vision-brownfield.md   # Feature in existing code
│   ├── vision-greenfield.md   # New project
│   └── operations/            # Production readiness
└── .aidlc-rule-details/       # Detailed methodology rules
```

## The Three Phases

| Phase | Focus | When Complete |
|-------|-------|---------------|
| 🔵 **Inception** | Understand & Design | Requirements and design approved |
| 🟢 **Construction** | Build & Test | Code complete, tests pass |
| 🟡 **Operations** | Ship & Maintain | Deployed or ready to deploy |

## Scaling to Your Task

| Task Size | Approach |
|-----------|----------|
| **Quick** (< 1 hour) | Mental checklist: think → test → code → verify → commit |
| **Standard** (1h - 1 day) | Lightweight docs: requirements + approach in one file |
| **Complex** (> 1 day) | Full AIDLC: formal requirements, design, state tracking |

## Skills

On-demand guidance for common workflows:

| Skill | Purpose |
|-------|---------|
| `secure-code` | Security hardening (Dockerfile, shell, API, database) |
| `pre-commit` | Quality checks before committing |
| `write-tests` | Test-driven development workflow |

Use `/skill-name` with Claude Code or reference directly.

## Key Principles

1. **Understand before building** — Don't assume requirements
2. **Design before coding** — Plan the approach
3. **Test alongside code** — Never skip tests
4. **Document decisions** — Track why, not just what
5. **Validate at gates** — Check before moving forward

## Files You'll Create

When using AIDLC, you'll create these in your project:

```
your-project/
└── aidlc-docs/
    ├── state.md           # Progress tracking
    ├── requirements.md    # Requirements + Q&A
    └── design.md          # Technical design
```

These are working documents — commit them for history, delete when done.

## Customization

### For Your Tech Stack

Copy and customize `templates/tech-env.md` to document:
- Languages and frameworks
- Build commands
- Code patterns
- Testing approach

### Adding Rules

Add custom rules to `.aidlc-rule-details/extensions/`:
- Security requirements
- Team conventions
- Project-specific guidelines

## References

- [AWS AI-DLC Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [AI-DLC GitHub](https://github.com/awslabs/aidlc-workflows)
- [Best Practices](docs/best-practices.md)
- [Pre-Commit Checklist](docs/pre-commit-checklist.md)

## License

This adaptation is provided as-is. Original AIDLC methodology by AWS Labs 
under Apache 2.0 license.
