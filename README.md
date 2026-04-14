# AIDLC Workshop

AI-Driven Development Life Cycle (AIDLC) workshop materials adapted from 
[AWS AIDLC](https://github.com/awslabs/aidlc-workflows) for structured 
development workshops.

## What is AIDLC?

AIDLC is a structured yet adaptive software development workflow that:

- **Analyzes requirements** and asks clarifying questions
- **Plans approaches** based on complexity and risk  
- **Adapts depth** — thorough for complex work, streamlined for simple changes
- **Documents decisions** with complete audit trail
- **Enforces quality gates** with checkpoints and approvals

## Quick Start

### For Your Project

1. Copy this repo into your project's `workshop/` directory (or use standalone)
2. Customize `templates/tech-env.md` for your stack
3. Fill in `inputs/<team>-vision.md` with your feature vision

### Starting a Session

```
Read AIDLC-WORKSHOP.md and follow the AIDLC workflow for this session.
```

### Resuming After a Break

```
Read AIDLC-WORKSHOP.md, then go to aidlc-docs/aidlc-state.md,
find the first unchecked item, and resume from that point.
```

## Directory Structure

```
aidlc-workshop/
├── AIDLC-WORKSHOP.md          # Main workflow (read this first)
├── FACILITATOR-GUIDE.md       # For workshop facilitators
├── README.md                  # This file
├── .aidlc-rule-details/       # AIDLC methodology rules
│   ├── common/                # Core workflow rules
│   │   ├── process-overview.md
│   │   ├── session-continuity.md
│   │   ├── question-format-guide.md
│   │   ├── content-validation.md
│   │   ├── depth-levels.md
│   │   ├── error-handling.md
│   │   ├── terminology.md
│   │   ├── welcome-message.md
│   │   ├── overconfidence-prevention.md
│   │   └── workflow-changes.md
│   ├── operations/
│   │   └── operations-overview.md
│   └── extensions/
│       └── security-baseline.md
├── templates/                 # Document templates
│   ├── vision-brownfield.md   # For adding to existing code
│   ├── vision-greenfield.md   # For new projects
│   ├── tech-env.md            # Technical environment (customize this)
│   ├── operations/            # Production readiness
│   │   ├── production-readiness.md
│   │   ├── deployment-checklist.md
│   │   └── observability-checklist.md
│   └── optional/              # Time-permitting stages
│       ├── user-stories.md
│       ├── units-generation.md
│       ├── nfr-requirements.md
│       ├── nfr-design.md
│       └── infrastructure-design.md
├── inputs/                    # Team input documents
│   └── .gitkeep
└── aidlc-docs/                # Generated during workshop
    └── .gitkeep
```

## The Three Phases

### 🔵 INCEPTION PHASE (Day 1)
**Focus**: WHAT to build and WHY

- Workspace Detection
- Reverse Engineering (brownfield)
- Requirements Analysis
- Cross-Team Alignment (workshops)
- Workflow Planning
- Application Design

### 🟢 CONSTRUCTION PHASE (Day 2)
**Focus**: HOW to build it

- Functional Design
- Code Generation (with tests!)
- Build and Test
- Integration

### 🟡 OPERATIONS PHASE (End of Day 2)
**Focus**: Production readiness

- Production Readiness Assessment
- Deployment Planning
- Post-Workshop Tasks

## Workshop Schedule

### Day 1: Inception Phase

| Time | Activity | Who |
|------|----------|-----|
| 9:00 | Kickoff, AIDLC overview | All |
| 9:30 | Workspace Detection | All |
| 10:00 | Reverse Engineering | All |
| 12:00 | Lunch | |
| 13:00 | Requirements Analysis | Per team |
| 14:30 | Cross-Team Alignment | All |
| 15:30 | Workflow Planning | Per team |
| 16:30 | Application Design | Per team |

### Day 2: Construction Phase

| Time | Activity | Who |
|------|----------|-----|
| 9:00 | Review Day 1, resolve questions | All |
| 9:30 | Functional Design | Per team |
| 10:30 | Code Generation | Per team |
| 12:00 | Lunch | |
| 13:00 | Continue Code Gen + Tests | Per team |
| 15:00 | Build and Test | Per team |
| 15:30 | Production Readiness Assessment | Per team |
| 16:00 | Integration | All |
| 16:30 | Demo + Retrospective | All |

## Key Rules

1. **Don't skip tests** — Generate tests alongside code
2. **Commit at gates** — `git commit` after each approval
3. **Clear context** — Start fresh after each stage
4. **No vibe coding** — Update design docs before changing code
5. **Own your files** — Each team edits only their components
6. **Questions in files** — Use `[Answer]:` tags, not chat responses
7. **Validate content** — Check diagrams and formatting before saving

## Customization

### For Your Tech Stack

Create or edit `templates/tech-env.md` to specify:
- Programming language and frameworks
- Build system and commands
- Code patterns and conventions
- Testing approach

### Adding Security Rules

Create extensions in `.aidlc-rule-details/extensions/`:
- Files without `.opt-in.md` suffix are always enforced
- Files with `.opt-in.md` suffix require explicit user opt-in

## References

- [AWS AI-DLC Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [AI-DLC GitHub](https://github.com/awslabs/aidlc-workflows)

## License

This adaptation is provided as-is for workshop use. Original AIDLC methodology 
by AWS Labs under Apache 2.0 license.
