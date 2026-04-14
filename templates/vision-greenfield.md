# Vision Document: [Project Name]

> **Template for greenfield projects** — Copy this to `workshop/inputs/<team>-vision.md`
> and fill in the sections.

## Team Information

| Field | Value |
|-------|-------|
| **Team Name** | |
| **Team Lead** | |
| **Workshop Date** | |

---

## Executive Summary

[One paragraph: What are you building, who is it for, why does it matter?]

Example:
> We're building a certificate rotation service that automatically renews device
> certificates before expiration. This prevents service disruptions caused by
> expired certificates, which currently require manual intervention and cause
> support escalations.

---

## Problem Statement

### The Problem

[What specific problem does this solve?]

### Who Experiences It

[Which users/teams/customers are affected?]

### Current Workarounds

[How is this handled today without this solution?]

### Cost of Inaction

[What happens if we don't build this?]

---

## Proposed Solution

### High-Level Approach

[Describe the solution at a conceptual level — what will it do?]

### Key Capabilities

1. [Capability 1]
2. [Capability 2]
3. [Capability 3]

### What This Is NOT

[Clarify scope by stating what the solution will NOT do]

---

## Target Users

| User Type | Description | Primary Need | Success Looks Like |
|-----------|-------------|--------------|-------------------|
| | | | |
| | | | |
| | | | |

---

## Success Metrics

| Metric | Baseline (Today) | Target | Timeframe | How Measured |
|--------|------------------|--------|-----------|--------------|
| | | | | |
| | | | | |

---

## Full Scope Vision

> Everything the product could become at maturity. This is aspirational —
> not all of this will be in the MVP.

### Feature Area 1: [Name]

| Feature | Description | Priority |
|---------|-------------|----------|
| | | |
| | | |

### Feature Area 2: [Name]

| Feature | Description | Priority |
|---------|-------------|----------|
| | | |
| | | |

### Feature Area 3: [Name]

| Feature | Description | Priority |
|---------|-------------|----------|
| | | |
| | | |

---

## MVP Scope — Features IN

> **Critical**: List every feature included in this workshop. If it's not listed,
> it's not in scope. Be ruthless — the MVP should be the smallest thing that
> delivers value.

| # | Feature | Description | Acceptance Criteria | Priority |
|---|---------|-------------|---------------------|----------|
| 1 | | | | Must Have |
| 2 | | | | Must Have |
| 3 | | | | Must Have |
| 4 | | | | Nice to Have |

### MVP Boundaries

**The MVP will:**
- [ ] ...
- [ ] ...

**The MVP will NOT:**
- [ ] ...
- [ ] ...

---

## MVP Scope — Features OUT

> Features deliberately excluded from MVP. This prevents scope creep and sets
> expectations with stakeholders.

| Feature | Why Excluded | Target Phase | Dependency |
|---------|--------------|--------------|------------|
| | Too complex for workshop | Phase 2 | |
| | Requires external integration | Future | |
| | Nice-to-have, not critical | Backlog | |

---

## Technical Considerations

### Proposed Architecture

[High-level architecture description — will be refined during Application Design]

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│   Service   │────▶│  Database   │
└─────────────┘     └─────────────┘     └─────────────┘
```

### Integration Points

| System | Integration Type | Purpose |
|--------|------------------|---------|
| | gRPC | |
| | REST | |
| | Message Queue | |

### Data Requirements

| Data Entity | Source | Storage | Retention |
|-------------|--------|---------|-----------|
| | | | |
| | | | |

### Non-Functional Requirements

| Category | Requirement | Target |
|----------|-------------|--------|
| Performance | Response time | < 200ms p99 |
| Availability | Uptime | 99.9% |
| Scalability | Concurrent users | 1000 |
| Security | Authentication | mTLS + JWT |

---

## Dependencies

### External Dependencies

| Dependency | Type | Owner | Risk Level |
|------------|------|-------|------------|
| | Service | | |
| | Library | | |
| | Infrastructure | | |

### Dependencies on Other Teams

| Dependency | Team | What We Need | When |
|------------|------|--------------|------|
| | | | |

### What Other Teams Need From Us

| Consumer | What They Need | Interface |
|----------|----------------|-----------|
| | | |

---

## Open Questions

> Things you already know are uncertain. These will be addressed during
> Requirements Analysis. The more you surface here, the faster that phase goes.

- [ ] 
- [ ] 
- [ ] 

---

## Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| | High/Med/Low | High/Med/Low | |
| | | | |
| | | | |

---

## Constraints

### Technical Constraints

- [ ] Must use [technology/pattern] because...
- [ ] Cannot use [technology/pattern] because...
- [ ] Must integrate with [system] via [method]

### Business Constraints

- [ ] Must be ready by [date] because...
- [ ] Budget limited to [X]
- [ ] Must comply with [regulation/policy]

### Organizational Constraints

- [ ] Team size: [X] people
- [ ] Available expertise: [list]
- [ ] Workshop time: 2 days

---

## Stakeholders

| Role | Name | Interest | Involvement |
|------|------|----------|-------------|
| Product Owner | | | Approve requirements |
| Tech Lead | | | Architecture decisions |
| Security | | | Security review |
| Ops | | | Deployment review |

---

## Appendix: Glossary

| Term | Definition |
|------|------------|
| | |
| | |
