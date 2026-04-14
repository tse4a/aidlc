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

**New here?** Read the [Getting Started Guide](GETTING-STARTED.md) for a complete walkthrough.

### 1. Install Superpowers Plugin

```bash
claude plugins install superpowers
```

### 2. Copy to Your Project

```bash
cp AIDLC.md CLAUDE.md GETTING-STARTED.md /path/to/your/project/
cp -r .claude /path/to/your/project/
mkdir -p /path/to/your/project/aidlc-docs
```

### 3. Start Building

```
/brainstorming

I want to build [description]
```

## Directory Structure

```
aidlc/
├── AIDLC.md                   # Main workflow (start here)
├── CLAUDE.md                  # Claude Code project instructions
├── .claude/
│   ├── settings.json          # Enables superpowers plugin
│   └── skills/                # Local skills (auto-loaded)
│       ├── secure-code/
│       ├── pre-commit/
│       ├── write-tests/
│       └── sync-upstream/     # Incorporate upstream updates
├── .github/
│   └── workflows/
│       └── check-upstream.yml # Weekly upstream sync check
├── docs/                      # Reference materials
│   ├── best-practices.md
│   └── pre-commit-checklist.md
├── templates/                 # Document templates
│   ├── tech-env.md
│   ├── vision-brownfield.md
│   ├── vision-greenfield.md
│   └── operations/
└── .aidlc-rule-details/       # Methodology rules
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

## Superpowers Plugin

This repo enables the **superpowers** plugin (`.claude/settings.json`) for enhanced workflows:

| Skill | Purpose | When to Use |
|-------|---------|-------------|
| `/brainstorming` | Turn ideas into designs | Before implementing complex features |
| `/writing-plans` | Create implementation plans | After brainstorming, before coding |
| `/debugging` | Systematic debugging | When troubleshooting issues |

Install superpowers: `claude plugins install superpowers`

## Local Skills

Project-specific skills in `.claude/skills/` with token-efficient structure:
- `SKILL.md` — Quick checklist, loaded automatically
- `references/` — Deep-dive guides, loaded on demand

| Skill | Purpose | Invoke |
|-------|---------|--------|
| `secure-code` | Security hardening | `/secure-code` |
| `pre-commit` | Quality checks | `/pre-commit` |
| `write-tests` | TDD workflow | `/write-tests` |
| `sync-upstream` | Incorporate AWS AIDLC updates | `/sync-upstream` |

Skills auto-trigger based on context (e.g., "security review" loads secure-code).

## Staying Updated

A GitHub Action checks the [upstream AWS AIDLC repo](https://github.com/awslabs/aidlc-workflows) weekly for updates. When changes are detected, it creates an issue. Use `/sync-upstream` to review and incorporate relevant changes.

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

- [Getting Started Guide](GETTING-STARTED.md) — Complete user guide
- [CLAUDE.md](CLAUDE.md) — How superpowers + AIDLC work together
- [AWS AI-DLC Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [AI-DLC GitHub](https://github.com/awslabs/aidlc-workflows)
- [Best Practices](docs/best-practices.md)
- [Pre-Commit Checklist](docs/pre-commit-checklist.md)

## License

This adaptation is provided as-is. Original AIDLC methodology by AWS Labs 
under Apache 2.0 license.
