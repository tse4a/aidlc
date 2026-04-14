# AIDLC Workshop Facilitator Guide

> **Audience**: Workshop facilitators running AIDLC sessions with NetSec teams
> 
> **Workshop Format**: 2 days, 2-3 teams, brownfield or greenfield SecureEdge features

---

## Table of Contents

1. [Pre-Workshop Preparation](#pre-workshop-preparation)
2. [Day 1: Inception Phase](#day-1-inception-phase)
3. [Day 2: Construction Phase](#day-2-construction-phase)
4. [Common Issues & Solutions](#common-issues--solutions)
5. [Cross-Team Coordination](#cross-team-coordination)
6. [Post-Workshop](#post-workshop)

---

## Pre-Workshop Preparation

### 2 Weeks Before

- [ ] **Identify participating teams** (2-3 from NetSec)
- [ ] **Define feature scope** — What will each team build?
- [ ] **Confirm brownfield vs greenfield** (likely brownfield for SecureEdge)
- [ ] **Reserve collaboration space** — Room with screens, or virtual meeting
- [ ] **Send pre-work instructions** (see email template below)

### 1 Week Before

- [ ] **Collect Vision documents** from each team
- [ ] **Review Vision documents** — Are they complete? Flag gaps early
- [ ] **Verify repo access** — All participants can clone/push
- [ ] **Test AI tools** — Claude Code / Cursor working for all participants
- [ ] **Copy workshop files** to target repo(s):
  ```bash
  cp -r workshop/ /path/to/target-repo/workshop/
  ```

### Day Before

- [ ] **Final Vision doc check** — All teams submitted?
- [ ] **Prep shared screen** for joint sessions
- [ ] **Print/share quick reference cards** (see appendix)
- [ ] **Dry run the first prompt**:
  ```
  Read workshop/AIDLC-WORKSHOP.md and follow the AIDLC workflow for this session.
  ```

---

## Pre-Work Email Template

```
Subject: AIDLC Workshop Prep - Action Required by [DATE]

Hi Team,

You're participating in the AIDLC Workshop on [DATES]. To make the most of our
time, please complete the following before the workshop:

1. **Create your Vision Document** (due [DATE - 3 days before])
   - Choose the right template:
     - **Brownfield** (adding to existing code): `workshop/templates/vision-brownfield.md`
     - **Greenfield** (new service): `workshop/templates/vision-greenfield.md`
   - Copy to: `workshop/inputs/[your-team]-vision.md`
   - Fill in all sections — especially MVP scope

2. **Review the Tech Environment** (pre-filled for SecureEdge)
   - File: workshop/templates/tech-env-secureedge.md
   - Customize if your feature needs different patterns

3. **Ensure tool access**
   - [ ] Can clone/push to the repo
   - [ ] Claude Code or Cursor installed and working
   - [ ] Reviewed AIDLC overview: https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/

4. **Identify dependencies**
   - Which other teams' features do you depend on?
   - What interfaces will you need to agree on?

Questions? Reply to this email or ping me on Slack.

See you at the workshop!
[Facilitator Name]
```

---

## Day 1: Inception Phase

### Opening (9:00 - 9:30)

**Facilitator Actions:**
1. Welcome participants, introductions if needed
2. Share agenda (on screen)
3. Explain AIDLC phases (5 min overview)
4. Set expectations:
   - "We'll produce working code, but not production-ready"
   - "Tests are mandatory — we learned this from the March workshop"
   - "Commit often — it's your safety net"

**Key Talking Points:**
> "AIDLC is about being deliberate. We plan before we code, we get approval
> before we proceed, and we document our decisions. This feels slower at first
> but prevents rework."

> "The AI is a collaborator, not an oracle. You review and approve everything.
> If something looks wrong, say so."

---

### Workspace Detection (9:30 - 10:00)

**All teams together**

**Facilitator Actions:**
1. One person drives (shared screen)
2. Run the opening prompt:
   ```
   Read workshop/AIDLC-WORKSHOP.md and follow the AIDLC workflow for this session.
   ```
3. AI will scan the codebase and report findings
4. Ensure everyone understands the codebase structure

**Watch For:**
- AI correctly identifies existing services
- No confusion about what's in scope vs out of scope

**Checkpoint:**
- [ ] `workshop/aidlc-docs/aidlc-state.md` created
- [ ] Everyone understands the codebase layout

---

### Reverse Engineering (10:00 - 12:00)

**All teams together**

**Facilitator Actions:**
1. Continue with AI analyzing existing code
2. Pause periodically to verify accuracy
3. Ask teams: "Does this match your understanding?"
4. Correct any misunderstandings immediately

**Guide the AI:**
> "Focus on the components relevant to our workshop features: [list them]"

> "Document the existing APIs we'll need to integrate with"

**Watch For:**
- AI making incorrect assumptions about code
- Missing important components
- Over-documenting irrelevant parts

**Checkpoint:**
- [ ] `workshop/aidlc-docs/inception/reverse-engineering/architecture.md` created
- [ ] Teams agree the documentation is accurate
- [ ] Component ownership is clear

**Commit Point:**
```bash
git add workshop/ && git commit -m "AIDLC Workshop: Reverse Engineering complete"
```

---

### Lunch (12:00 - 13:00)

Encourage teams to informally discuss their features.

---

### Requirements Analysis (13:00 - 14:30)

**Teams split** — Each team works independently

**Facilitator Actions:**
1. Float between teams
2. Ensure each team loads their Vision document:
   ```
   Please read workshop/inputs/[team]-vision.md and continue with Requirements Analysis.
   ```
3. Help teams answer clarifying questions
4. Watch the clock — 45 min per team max

**Guide Teams:**
> "Be specific in your answers. 'It depends' is not an answer — pick one path
> for the workshop and note alternatives as future work."

> "If a question seems irrelevant, say so: 'This is out of scope for the workshop.'"

**Watch For:**
- Teams getting stuck on edge cases (redirect to MVP)
- Scope creep ("while we're at it...")
- Missing the "What Must NOT Change" constraints

**Checkpoint (per team):**
- [ ] Clarifying questions answered
- [ ] `workshop/aidlc-docs/inception/requirements/[team]-requirements.md` created
- [ ] Team approves requirements

---

### Cross-Team Alignment (14:30 - 15:30)

**All teams together** — Workshop-specific stage

**Facilitator Actions:**
1. Bring teams back together
2. Each team presents (5-10 min each):
   - What they're building
   - What they need from other teams
   - What they're providing to other teams
3. Facilitate interface discussions
4. Document agreements

**Key Questions to Ask:**
- "Team A, you need user data from Team B's service — what's the API contract?"
- "Are there any conflicting assumptions?"
- "What happens if Team B's feature isn't ready when Team A needs it?"

**Document Agreements:**
Create `workshop/aidlc-docs/inception/cross-team-contracts.md`:
```markdown
# Cross-Team Interface Contracts

## Team A ↔ Team B

### API: GetUserDevices
- **Provider**: Team B
- **Consumer**: Team A
- **Contract**:
  ```protobuf
  rpc GetUserDevices(GetUserDevicesRequest) returns (GetUserDevicesResponse)
  ```
- **Agreed**: [timestamp]
```

**Checkpoint:**
- [ ] All dependencies identified
- [ ] Interface contracts documented
- [ ] No blocking conflicts

---

### Workflow Planning (15:30 - 16:00)

**Per team**

**Facilitator Actions:**
1. Teams return to independent work
2. AI creates execution plan for Day 2
3. Ensure plans are realistic for remaining time

**Guide Teams:**
> "Be honest about what you can finish tomorrow. It's better to complete 3
> features well than start 5 and finish none."

**Checkpoint:**
- [ ] Each team has approved plan
- [ ] Plans account for integration time

---

### Application Design (16:00 - 17:00)

**Per team, if needed**

**Facilitator Actions:**
1. Only needed if new components/services are being created
2. Simple feature additions may skip this
3. Ensure designs align with cross-team contracts

**End of Day 1 Wrap-Up (16:45):**
1. Quick round-robin: "What did you accomplish? Any blockers?"
2. Preview Day 2 schedule
3. Remind: "Review your design tonight if possible"

**Day 1 Commit:**
```bash
git add workshop/ && git commit -m "AIDLC Workshop Day 1: Inception complete"
git push
```

---

## Day 2: Construction Phase

### Morning Standup (9:00 - 9:30)

**Facilitator Actions:**
1. Quick sync: "Any overnight thoughts or concerns?"
2. Review Day 2 plan
3. Remind teams of integration time at 15:00
4. Emphasize: **Tests are mandatory**

**Key Message:**
> "Today we write code. Remember: update the design if you discover issues,
> don't just hack around them. And write tests — the March workshop skipped
> tests and it created debt."

---

### Functional Design (9:30 - 10:00)

**Per team, if needed**

Quick detailed design for complex business logic. Skip for simple features.

---

### Code Generation (10:00 - 12:00 and 13:00 - 15:00)

**Per team** — This is the main work

**Facilitator Actions:**
1. Float between teams
2. Watch for common issues (see troubleshooting)
3. Ensure tests are being written
4. Check progress at 11:00, 14:00

**Remind Teams:**
```
At each code generation step:
1. Review the plan before approving
2. Check generated code compiles
3. Verify tests are included
4. Commit after each unit
```

**Watch For:**
- AI generating code in wrong location (should be workspace root, not workshop/)
- Missing tests
- Security rule violations (check against secureedge-security.md)
- Teams falling behind

**If a Team Falls Behind:**
- Help them scope down: "What's the minimum viable feature?"
- Offer to skip optional stages
- Pair with another team member to parallelize

**Checkpoints:**
- [ ] 11:00 — At least one unit code-generated
- [ ] 14:00 — Core functionality generated
- [ ] 15:00 — Tests written and passing

---

### Lunch (12:00 - 13:00)

---

### Build and Test (15:00 - 15:30)

**Per team**

**Facilitator Actions:**
1. Each team runs:
   ```bash
   make build
   make test
   make lint
   ```
2. Help debug failures
3. Ensure all tests pass before moving on

**Common Issues:**
- Import errors → Check package paths
- Test failures → Read the error, fix the test or code
- Lint errors → `make lint-fix`

---

### Production Readiness Assessment (15:30 - 16:00)

**Per team** — Operations Phase begins

**Facilitator Actions:**
1. Distribute `workshop/templates/operations/production-readiness.md`
2. Teams assess each category: Code, Testing, Security, Docs, Observability, Deployment
3. Mark status: 🔴 Blocked | 🟡 In Progress | 🟢 Complete
4. Identify post-workshop tasks

**Guide Teams:**
> "Be honest about what's not ready. The goal is to leave with a clear list of
> what needs to happen before this can ship."

**Key Questions:**
- "What's blocking production deployment?"
- "Who owns each gap?"
- "What's the realistic timeline?"

**Checkpoint:**
- [ ] Production readiness checklist filled out
- [ ] Blocking issues identified
- [ ] Post-workshop tasks listed

---

### Integration (16:00 - 16:30)

**All teams together**

**Facilitator Actions:**
1. Bring teams together
2. Connect the interfaces defined in cross-team contracts
3. Run integration tests if available
4. Document any issues for post-workshop

**If Conflicts Arise:**
> "Let's document this conflict and pick ONE solution for the demo. We can
> revisit after the workshop."

---

### Demo & Retrospective (16:30 - 17:30)

**All teams together**

**Demo (20 min):**
1. Each team demos their feature (5-7 min each)
2. Show the working code, not just slides
3. Highlight what AIDLC helped with

**Retrospective (25 min):**
Ask:
- What worked well with AIDLC?
- What was frustrating?
- Would you use this approach again?
- What would you change?

**Capture Feedback:**
Create `workshop/aidlc-docs/retrospective.md` with responses.

**Final Commit:**
```bash
git add workshop/ && git commit -m "AIDLC Workshop Day 2: Construction + Operations complete"
git push
```

---

## Operations Phase Templates

Three templates support the Operations phase:

### Production Readiness (`templates/operations/production-readiness.md`)
- Comprehensive checklist covering Code, Testing, Security, Docs, Observability, Deployment
- Status tracking: 🔴 Blocked | 🟡 In Progress | 🟢 Complete
- Sign-off section for stakeholders
- Post-workshop task list

### Deployment Checklist (`templates/operations/deployment-checklist.md`)
- Pre-deployment verification
- Helm chart changes
- Feature flag configuration
- Rollout strategy
- Rollback procedures

### Observability Checklist (`templates/operations/observability-checklist.md`)
- Metrics implementation
- Tracing setup
- Logging standards
- Dashboard creation
- Alert configuration

---

## Common Issues & Solutions

### AI Generates Code in Wrong Location

**Symptom**: Code appears in `workshop/aidlc-docs/` instead of workspace root

**Solution**:
> "Application code should go in the workspace root (e.g., internal/, cmd/),
> not in workshop/aidlc-docs/. Please move [file] to [correct location]."

---

### AI Skips Tests

**Symptom**: Code generated without corresponding tests

**Solution**:
> "Before approving this code generation, please also generate unit tests.
> Follow the patterns in templates/tech-env-secureedge.md."

---

### Team Gets Stuck on Requirements

**Symptom**: Endless clarifying questions, no progress

**Solution**:
1. Timebox: "You have 10 more minutes, then we proceed with what we have"
2. Make decisions: "For the workshop, let's assume [X]. Note [Y] as future work."
3. Skip optional questions: "Mark this as out of scope"

---

### Context Gets Confused

**Symptom**: AI references wrong files, forgets prior decisions

**Solution**:
1. Clear context: `/clear` or new chat
2. Resume properly:
   ```
   Read workshop/AIDLC-WORKSHOP.md, then go to workshop/aidlc-docs/aidlc-state.md,
   find the first unchecked item, and resume from that point.
   ```

---

### Security Violations in Generated Code

**Symptom**: Code uses string concatenation for SQL, hardcoded secrets, etc.

**Solution**:
> "This code violates our security rules. Please review
> workshop/.aidlc-rule-details/extensions/barracuda/secureedge-security.md
> and regenerate with proper [parameterized queries / environment variables / etc.]"

---

### Teams Have Conflicting Changes

**Symptom**: Two teams modified the same file

**Solution**:
1. Don't merge yet
2. Review together:
   > "Review all changes across teams and report any conflicts. Do not edit —
   > produce a report for our review."
3. Decide which change wins, or merge manually
4. Document decision in audit.md

---

## Cross-Team Coordination

### Before Teams Split

Establish clear ownership:

```markdown
## File Ownership

| Path | Owner |
|------|-------|
| internal/service/device/ | Team A |
| internal/service/user/ | Team B |
| internal/handler/ | Shared (coordinate) |
| api/proto/*.proto | Shared (coordinate) |
```

### During Independent Work

- Teams edit ONLY their owned files
- Shared files require coordination (Slack/call before editing)
- Proto changes must be agreed by all affected teams

### Before Integration

Run conflict check:
```
Review all files modified by any team today. Report any files touched by
multiple teams, and any conflicts between changes.
```

---

## Post-Workshop

### Immediate (Day of)

- [ ] Push all code to remote
- [ ] Create summary PR or branch
- [ ] Share retrospective notes

### Within 1 Week

- [ ] Teams create Jira tickets for remaining work
- [ ] Identify what's ready for PR vs needs more work
- [ ] Schedule follow-up if needed

### Track Production Path

From March workshop experience: workshop code → production takes ~7 weeks.

Use the production readiness checklist completed during the workshop to track:
- [ ] Address all 🔴 Blocked items
- [ ] Complete all 🟡 In Progress items
- [ ] Split workshop branch into reviewable PRs
- [ ] Execute deployment checklist
- [ ] Configure observability (dashboards, alerts)
- [ ] Roll out via feature flags

---

## Quick Reference Card

Print/share this with participants:

```
┌─────────────────────────────────────────────────────────────┐
│                  AIDLC WORKSHOP QUICK REF                   │
├─────────────────────────────────────────────────────────────┤
│ START SESSION:                                              │
│   Read workshop/AIDLC-WORKSHOP.md and follow the AIDLC     │
│   workflow for this session.                                │
│                                                             │
│ RESUME AFTER BREAK:                                         │
│   Read workshop/AIDLC-WORKSHOP.md, then go to              │
│   workshop/aidlc-docs/aidlc-state.md, find the first       │
│   unchecked item, and resume from that point.              │
│                                                             │
│ EXPLORE WITHOUT CHANGING:                                   │
│   Do not update any documents. Help me understand [X].     │
│                                                             │
│ COMMIT AT GATES:                                            │
│   git add workshop/ && git commit -m "AIDLC: [stage]"      │
│                                                             │
│ KEY RULES:                                                  │
│   ✓ Tests are mandatory                                    │
│   ✓ Update design before changing code                     │
│   ✓ Clear context at each approval gate                    │
│   ✓ Code goes in workspace root, not workshop/             │
│   ✓ Only edit files your team owns                         │
└─────────────────────────────────────────────────────────────┘
```

---

## Appendix: Timing Cheat Sheet

| Phase | Stage | Duration | Hard Stop |
|-------|-------|----------|-----------|
| Inception | Workspace Detection | 30 min | 10:00 |
| Inception | Reverse Engineering | 2 hr | 12:00 |
| Inception | Requirements (per team) | 1.5 hr | 14:30 |
| Inception | Cross-Team Alignment | 1 hr | 15:30 |
| Inception | Workflow Planning | 30 min | 16:00 |
| Inception | Application Design | 1 hr | 17:00 |
| Construction | Code Generation | 4 hr | 15:00 (Day 2) |
| Construction | Build & Test | 30 min | 15:30 (Day 2) |
| Operations | Production Readiness | 30 min | 16:00 (Day 2) |
| Operations | Integration | 30 min | 16:30 (Day 2) |
| - | Demo & Retro | 1 hr | 17:30 (Day 2) |

If falling behind, cut:
1. Application Design (if simple features)
2. Functional Design (if clear requirements)
3. Scope down features (keep tests!)
4. Do NOT skip Production Readiness — it's how you track post-workshop work

---

## Appendix: Optional Stage Templates

If time permits, these additional templates can enhance specific areas:

| Template | Location | When to Use |
|----------|----------|-------------|
| User Stories | `templates/optional/user-stories.md` | User-facing features with multiple personas |
| Units Generation | `templates/optional/units-generation.md` | Work that can be parallelized across team members |
| NFR Requirements | `templates/optional/nfr-requirements.md` | Performance, scalability, security requirements |
| NFR Design | `templates/optional/nfr-design.md` | Caching, circuit breaker, resilience patterns |
| Infrastructure Design | `templates/optional/infrastructure-design.md` | K8s resources, database changes, external dependencies |

**When to suggest optional templates:**
- Team asks "how do we handle performance requirements?" → NFR Requirements + NFR Design
- Team has parallelizable work → Units Generation
- Team building user-facing UI → User Stories
- Team deploying new services → Infrastructure Design
