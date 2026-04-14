# AIDLC Workshop Workflow

> **Workshop Mode**: This file provides the AI-DLC workflow for structured workshop
> development. Invoke this explicitly when running a workshop session.
>
> **To start**: Tell the AI "Read AIDLC-WORKSHOP.md and follow the AIDLC
> workflow for this session."

---

## Workshop Context

This workflow is adapted from [AWS AI-DLC](https://github.com/awslabs/aidlc-workflows)
for structured workshop development. It provides phases for brownfield or greenfield
feature development, optionally across multiple collaborating teams.

**Workshop Duration**: 2 days  
**Project Type**: Brownfield or Greenfield  
**Teams**: 1 or more teams collaborating on related features

---

## Pre-Workshop Checklist

Before starting AIDLC, each team must have:

- [ ] **Vision Document** — `inputs/<team>-vision.md`
  - Use `templates/vision-brownfield.md` for adding features to existing code
  - Use `templates/vision-greenfield.md` for new projects from scratch
- [ ] **Technical Environment** — `inputs/<team>-tech-env.md` (customize from `templates/tech-env.md`)
- [ ] **Identified shared touchpoints** with other teams (if multi-team)

Templates are in `templates/`.

---

## AIDLC Three-Phase Workflow

```
                         Team Request
                              │
                              ▼
        ┌───────────────────────────────────────┐
        │     🔵 INCEPTION PHASE                │
        │     Planning & Application Design     │
        ├───────────────────────────────────────┤
        │ • Workspace Detection (ALWAYS)        │
        │ • Reverse Engineering (BROWNFIELD)    │
        │ • Requirements Analysis (ALWAYS)      │
        │ • Cross-Team Alignment (MULTI-TEAM)   │
        │ • Workflow Planning (ALWAYS)          │
        │ • Application Design (CONDITIONAL)    │
        └───────────────────────────────────────┘
                              │
                              ▼
        ┌───────────────────────────────────────┐
        │     🟢 CONSTRUCTION PHASE             │
        │     Design, Implementation & Test     │
        ├───────────────────────────────────────┤
        │ • Functional Design (CONDITIONAL)     │
        │ • Code Generation (ALWAYS)            │
        │ • Tests (MANDATORY - don't skip!)     │
        │ • Build and Test (ALWAYS)             │
        └───────────────────────────────────────┘
                              │
                              ▼
        ┌───────────────────────────────────────┐
        │     🟡 OPERATIONS PHASE               │
        │     Production Readiness & Planning   │
        ├───────────────────────────────────────┤
        │ • Production Readiness (ALWAYS)       │
        │ • Deployment Planning (ALWAYS)        │
        │ • Post-Workshop Tasks (ALWAYS)        │
        └───────────────────────────────────────┘
                              │
                              ▼
                          Complete
```

---

## Day 1: Inception Phase

### Stage 1: Workspace Detection (30 min)

**All teams together**

1. Load rule details from `.aidlc-rule-details/common/process-overview.md`
2. Scan workspace for existing code structure
3. Identify: programming languages, build system, project structure
4. Create `aidlc-docs/aidlc-state.md` to track progress

**Output**: Shared understanding of codebase state

### Stage 2: Reverse Engineering (1-2 hours)

**All teams together** — Build shared mental model (skip for greenfield)

1. Analyze existing codebase architecture
2. Document components, APIs, data flows
3. Identify boundaries between team ownership areas
4. Create `aidlc-docs/inception/reverse-engineering/`:
   - `architecture.md` — System overview
   - `component-inventory.md` — What exists
   - `api-documentation.md` — Existing endpoints

**Output**: Shared architecture documentation

### Stage 3: Requirements Analysis (1-2 hours per team)

**Teams split** — Each team defines their scope

1. Load team's Vision document from `inputs/`
2. Analyze request clarity, type, scope, complexity
3. Create clarifying questions in `aidlc-docs/inception/requirements/<team>-questions.md`
4. Team answers questions using `[Answer]:` tags
5. Generate `aidlc-docs/inception/requirements/<team>-requirements.md`

**Gate**: Team approves requirements before proceeding

### Stage 4: Cross-Team Alignment (1 hour)

**All teams together** — Multi-team workshop stage (skip for single team)

1. Each team presents their requirements
2. Identify dependencies and shared interfaces
3. Agree on API contracts between teams
4. Document in `aidlc-docs/inception/cross-team-contracts.md`

**Output**: Agreed interface contracts

### Stage 5: Workflow Planning (30 min per team)

1. Determine which Construction stages are needed
2. Create execution plan
3. Estimate scope for Day 2

**Gate**: Team approves plan

### Stage 6: Application Design (1 hour per team, if needed)

1. Design new components/services
2. Define component methods and interactions
3. Document in `aidlc-docs/inception/application-design/`

**End of Day 1**: All teams have approved designs

---

## Day 2: Construction Phase

### Stage 7: Functional Design (30 min per unit, if needed)

1. Detail business logic for each unit
2. Define data models, schemas
3. Document in `aidlc-docs/construction/<unit>/functional-design.md`

### Optional Stages (Time Permitting)

The following stages may be added if time allows. Templates are in `templates/optional/`:

| Stage | Template | When to Use |
|-------|----------|-------------|
| User Stories | `user-stories.md` | User-facing features with multiple personas |
| Units Generation | `units-generation.md` | Parallelizable work across team members |
| NFR Requirements | `nfr-requirements.md` | Performance, scalability, security requirements |
| NFR Design | `nfr-design.md` | Caching, resilience, observability patterns |
| Infrastructure Design | `infrastructure-design.md` | K8s resources, database infra, external deps |

### Stage 8: Code Generation (2-3 hours)

**Part 1 - Planning**:
1. Create detailed code generation plan with checkboxes
2. Get team approval

**Part 2 - Generation**:
1. Generate code following plan
2. Check off each step as completed
3. **MANDATORY**: Generate tests alongside code

**Files go in workspace root, NOT in aidlc-docs/**

### Stage 9: Build and Test (1 hour)

1. Run build command
2. Run tests — all tests must pass
3. Run linters — fix any issues
4. Document results in `aidlc-docs/construction/build-and-test/`

### Stage 10: Integration (1 hour)

**All teams together** (skip for single team)

1. Wire up cross-team interfaces
2. Test integration points
3. Resolve any conflicts

### Stage 11: Demo Prep (30 min)

1. Prepare demo of working feature
2. Document what was built vs. what remains

---

## Day 2: Operations Phase (End of Day)

### Stage 12: Production Readiness Assessment (30 min)

**Per team**

1. Complete `templates/operations/production-readiness.md`
2. Identify gaps blocking production
3. Categorize: 🔴 Blocked | 🟡 In Progress | 🟢 Complete

### Stage 13: Deployment Planning (15 min)

1. Review `templates/operations/deployment-checklist.md`
2. Identify config/infrastructure changes needed
3. Define feature flag strategy

### Stage 14: Post-Workshop Tasks (15 min)

1. Create tickets for remaining work
2. Assign owners and priorities
3. Document in `aidlc-docs/operations/post-workshop-tasks.md`

**Templates in**: `templates/operations/`

---

## Workshop Rules

### Context Management

Clear context at every approval gate:
```
[Complete stage] → [Approve] → [git commit] → [Clear context] → [Resume]
```

To resume after context clear:
> "Go to aidlc-docs/aidlc-state.md, find the first unchecked item,
> then resume from that point."

### Exploratory Questions

When exploring without changing docs:
> "Do not update any documents. Help me understand [question]."

### Never Vibe Code

If you find an issue in generated code:
1. Go back to the design document
2. Update the design
3. Regenerate the code

Do NOT directly edit generated code without updating design first.

### Cross-Team File Ownership

Each team restricts edits to their owned files. When all teams are done:
> "Review all changes across teams and report any conflicts. Do not edit —
> produce a report for our review."

---

## Security Extensions

The following security rules are enforced (see `.aidlc-rule-details/extensions/security-baseline.md`):

All generated code must comply with:
- Input validation for all external inputs
- Parameterized queries (no SQL string concatenation)
- Secrets from environment only
- Non-root containers
- Error messages don't leak internals
- Timeout contexts for external calls

### Testing (Mandatory)

- Unit tests required for all new code
- Aim for 80% coverage on new code
- Integration tests for cross-service calls

---

## Directory Structure

```
aidlc-workshop/
├── AIDLC-WORKSHOP.md              # This file
├── .aidlc-rule-details/           # AIDLC detailed rules
│   ├── common/
│   ├── operations/
│   └── extensions/
├── inputs/                        # Team input documents
│   ├── team-a-vision.md
│   ├── team-a-tech-env.md
│   └── ...
├── templates/                     # Document templates
│   ├── vision-brownfield.md
│   ├── vision-greenfield.md
│   ├── tech-env.md
│   ├── operations/
│   │   ├── production-readiness.md
│   │   ├── deployment-checklist.md
│   │   └── observability-checklist.md
│   └── optional/
│       ├── user-stories.md
│       ├── units-generation.md
│       ├── nfr-requirements.md
│       ├── nfr-design.md
│       └── infrastructure-design.md
└── aidlc-docs/                    # Generated during workshop
    ├── aidlc-state.md             # Progress tracking
    ├── audit.md                   # Decision log
    └── inception/
        ├── reverse-engineering/
        ├── requirements/
        └── application-design/
```

---

## Quick Reference

| Phase | Stage | Duration | Gate |
|-------|-------|----------|------|
| Inception | Workspace Detection | 30m | Auto |
| Inception | Reverse Engineering | 1-2h | Approval |
| Inception | Requirements Analysis | 1-2h | Approval |
| Inception | Cross-Team Alignment | 1h | Approval |
| Inception | Workflow Planning | 30m | Approval |
| Inception | Application Design | 1h | Approval |
| Construction | Functional Design | 30m | Approval |
| Construction | Code Generation | 2-3h | Approval |
| Construction | Build and Test | 1h | Pass |
| Construction | Integration | 1h | Pass |

---

## Getting Help

- **AIDLC methodology**: https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/
- **Full AIDLC rules**: https://github.com/awslabs/aidlc-workflows
