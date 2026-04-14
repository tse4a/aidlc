# Operations Phase Overview

**Purpose**: Guide teams from workshop prototype to production deployment.

---

## When to Use Operations Phase

The Operations phase bridges the gap between "working code" and "production-ready code."

**During Workshop (Day 2 end)**:
- Complete Production Readiness Checklist
- Identify post-workshop work items
- Create deployment plan outline

**Post-Workshop**:
- Execute full deployment pipeline
- Configure monitoring and alerts
- Roll out with feature flags

---

## Operations Stages

### 1. Production Readiness Assessment

Evaluate what's needed before code can ship:

- [ ] Code review complete
- [ ] Test coverage adequate (80%+ on new code)
- [ ] Security review passed
- [ ] Documentation updated
- [ ] Feature flag configured
- [ ] Rollback plan defined

**Template**: `workshop/templates/operations/production-readiness.md`

### 2. Deployment Planning

Define how the code gets to production:

- [ ] Helm chart changes identified
- [ ] Environment configs updated
- [ ] Database migrations planned
- [ ] Dependency updates documented
- [ ] CI/CD pipeline verified

**Template**: `workshop/templates/operations/deployment-checklist.md`

### 3. Observability Setup

Ensure you can monitor the new feature:

- [ ] Metrics exposed
- [ ] Traces connected
- [ ] Logs structured
- [ ] Dashboard created/updated
- [ ] Alerts configured

**Template**: `workshop/templates/operations/observability-checklist.md`

### 4. Rollout Strategy

Plan the production rollout:

- [ ] Feature flag targeting defined
- [ ] Rollout percentage schedule
- [ ] Success criteria defined
- [ ] Rollback triggers identified
- [ ] On-call notified

---

## Workshop vs Post-Workshop

| Activity | Workshop (Day 2) | Post-Workshop |
|----------|------------------|---------------|
| Production Readiness Checklist | Complete assessment | Address gaps |
| Deployment Planning | Outline plan | Execute deployment |
| Observability | Identify needs | Implement dashboards/alerts |
| Rollout | Define strategy | Execute rollout |

---

## Output Artifacts

Operations phase produces:

```
workshop/aidlc-docs/operations/
├── production-readiness.md      # Filled checklist with gaps identified
├── deployment-plan.md           # How to deploy
├── post-workshop-tasks.md       # Jira tickets / work items for completion
└── rollout-plan.md              # Feature flag and rollout strategy
```

---

## Integration with Construction

Operations begins after Build and Test passes:

```
Construction: Build & Test
         │
         ▼ (tests pass)
Operations: Production Readiness Assessment
         │
         ▼
Operations: Deployment Planning
         │
         ▼
Operations: Identify Post-Workshop Work
         │
         ▼
Workshop Complete → Post-Workshop Execution
```
