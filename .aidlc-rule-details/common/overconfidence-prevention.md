# Overconfidence Prevention Guide

## Problem Statement

AI assistants can exhibit overconfidence by not asking enough clarifying questions, making assumptions instead of gathering proper requirements. This leads to:

- Wrong implementations based on assumptions
- Rework when requirements are clarified later
- User frustration from misunderstood intent

---

## Core Principle

**When in doubt, ask the question.**

The cost of asking one extra clarifying question is far less than the cost of implementing the wrong solution based on assumptions.

---

## Red Flags: Signs of Overconfidence

### During Requirements Analysis

| Red Flag | What to Do Instead |
|----------|---------------------|
| Completing stage without asking any questions | Always ask at least 2-3 clarifying questions |
| Accepting vague answers ("it depends", "maybe") | Create follow-up questions to clarify |
| Making assumptions about scope | Ask explicitly about boundaries |
| Skipping entire question categories | Evaluate ALL categories, skip only with justification |

### During Design

| Red Flag | What to Do Instead |
|----------|---------------------|
| Jumping to implementation details | Confirm high-level approach first |
| Assuming technology choices | Ask about constraints and preferences |
| Not asking about error cases | Always clarify error handling expectations |
| Skipping security considerations | Ask about authentication, authorization, data sensitivity |

### During Code Generation

| Red Flag | What to Do Instead |
|----------|---------------------|
| Generating large amounts without checkpoints | Break into smaller approved chunks |
| Not confirming patterns match existing code | Show examples and ask for confirmation |
| Assuming test coverage expectations | Ask about required test types and coverage |

---

## Question Generation Philosophy

### Old Approach (Avoid)

> "Only ask questions if absolutely necessary"
> "Skip entire categories if not applicable"
> "Use categories as inspiration, NOT as mandatory checklist"

### New Approach (Follow)

> "When in doubt, ask the question"
> "Evaluate ALL categories, explain why any are skipped"
> "Better to over-clarify than under-clarify"

---

## Mandatory Question Categories

For each stage, evaluate these areas and ask questions where unclear:

### Requirements Analysis

- [ ] Functional requirements — what should it do?
- [ ] Non-functional requirements — how should it perform?
- [ ] Business context — why is this needed?
- [ ] Technical context — what exists already?
- [ ] Constraints — what are the boundaries?
- [ ] Success criteria — how will we know it works?

### Design Stages

- [ ] Data flow — how does data move through the system?
- [ ] Integration points — what does this connect to?
- [ ] Error handling — what happens when things fail?
- [ ] Security — authentication, authorization, data protection?
- [ ] Scalability — how will this grow?
- [ ] Observability — how will we monitor this?

### Code Generation

- [ ] Patterns — does this match existing code?
- [ ] Tests — what types are required?
- [ ] Edge cases — what unusual scenarios exist?
- [ ] Dependencies — what libraries/services are needed?

---

## Detecting Vague Responses

Watch for these patterns in user answers:

| Vague Response | Follow-Up Action |
|----------------|------------------|
| "It depends" | Ask: "What does it depend on? Let's enumerate the cases." |
| "Maybe" / "Possibly" | Ask: "What would make it yes vs. no?" |
| "Not sure" | Ask: "Who would know? Should we proceed with assumption X?" |
| "Mix of both" | Ask: "Can you give percentages or primary vs. secondary?" |
| "Somewhere between" | Ask: "Can you pick the closer option for now?" |
| "Similar to X" | Ask: "What's the same and what's different?" |

---

## Contradiction Detection

After receiving answers, check for logical inconsistencies:

| Contradiction Type | Example |
|--------------------|---------|
| Scope vs. Impact | "Bug fix" but "affects entire codebase" |
| Risk vs. Changes | "Low risk" but "breaking API changes" |
| Timeline vs. Complexity | "Quick fix" but "multiple services involved" |
| Single component vs. Architecture | "One service change" but "new database schema" |

When contradictions found:

1. Create a clarification question file
2. Explain the contradiction clearly
3. Ask which interpretation is correct
4. **Do NOT proceed until resolved**

---

## Follow-Up Question Triggers

Create follow-up questions when:

- [ ] Any answer contains vague language
- [ ] Answer references undefined terms
- [ ] Answer contradicts a previous answer
- [ ] Answer raises new questions
- [ ] Answer is shorter than expected for a complex topic
- [ ] Answer mentions "later" or "eventually" for scope decisions

---

## Workshop Context

In workshops with time pressure, the temptation to skip questions increases. Resist this.

**Time-saving approaches that maintain quality:**

| Instead of | Do This |
|------------|---------|
| Skipping questions | Ask fewer but more targeted questions |
| Accepting vague answers | Offer concrete options: "A, B, or C?" |
| Making assumptions | State the assumption and ask for confirmation |
| Proceeding with ambiguity | Time-box the clarification: "Let's decide in 2 minutes" |

---

## Quality Assurance

### After Each Stage, Verify

- [ ] Did I ask at least 2-3 clarifying questions?
- [ ] Did I analyze ALL answers for ambiguity?
- [ ] Did I create follow-ups for vague responses?
- [ ] Did I resolve any contradictions?
- [ ] Am I confident about the requirements?

### If Unsure

Ask one more question. The user can always say "that's clear enough, proceed."

---

## Key Takeaway

**It's better to ask too many questions than to make incorrect assumptions.**

A 2-minute clarification now prevents a 2-hour rework later.
