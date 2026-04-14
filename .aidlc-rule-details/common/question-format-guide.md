# Question Format Guide

## Rule: Questions Go in Files, Not Chat

**CRITICAL**: Never ask questions directly in chat. All questions must be placed in dedicated question files.

---

## Question File Format

### File Naming

Use descriptive names in the appropriate directory:
- `aidlc-docs/inception/requirements/<team>-questions.md`
- `aidlc-docs/inception/user-stories/<team>-story-questions.md`
- `aidlc-docs/construction/<unit>/design-questions.md`

### Question Structure

Every question must include meaningful options plus "Other" as the last option:

```markdown
## Question [Number]
[Clear, specific question text]

A) [First meaningful option]
B) [Second meaningful option]
C) [Additional options as needed]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

**Rules:**
- "Other" is MANDATORY as the LAST option
- Only include meaningful options — don't make up options to fill slots
- Minimum: 2 meaningful options + Other
- Maximum: 5 meaningful options + Other

---

## Complete Example

```markdown
# Requirements Clarification Questions

Please answer by filling in the letter after each [Answer]: tag.

## Question 1
What is the primary authentication method for this feature?

A) Existing SecureEdge device certificates
B) User SSO via Azure AD
C) API key authentication
D) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 2
What is the expected request volume?

A) Low (< 100 requests/minute)
B) Medium (100-1000 requests/minute)
C) High (> 1000 requests/minute)
D) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 3
Is this a breaking change to existing APIs?

A) No, purely additive
B) Yes, but backward compatible
C) Yes, breaking change with migration path
D) Other (please describe after [Answer]: tag below)

[Answer]: 
```

---

## User Response Format

Users answer by filling in the letter choice:

```markdown
## Question 1
What is the primary authentication method?

A) Existing SecureEdge device certificates
B) User SSO via Azure AD
C) API key authentication
D) Other (please describe after [Answer]: tag below)

[Answer]: A
```

For "Other" responses:

```markdown
[Answer]: D - We need both device certs AND user SSO for this feature
```

---

## Workflow

### Step 1: Create Question File

```markdown
Create aidlc-docs/inception/requirements/<team>-questions.md with all questions
```

### Step 2: Inform Team

```
I've created <team>-questions.md with [X] questions.
Please answer each question by filling in the letter after the [Answer]: tag.
Let me know when you're done.
```

### Step 3: Wait for Confirmation

Wait for user to say "done", "completed", "finished", or similar.

### Step 4: Read and Validate

1. Read the question file
2. Check all [Answer]: tags are filled
3. Validate answers are valid letter choices
4. Check for contradictions (see below)
5. Proceed with analysis

---

## Contradiction Detection

**MANDATORY**: After reading responses, check for contradictions.

### Common Contradictions

| Conflict | Example |
|----------|---------|
| Scope vs Impact | "Bug fix" but "Affects entire codebase" |
| Risk vs Changes | "Low risk" but "Breaking API changes" |
| Timeline vs Complexity | "Quick fix" but "Multiple services involved" |
| Single component vs Architecture | "One service change" but "New database schema" |

### When Contradictions Found

Create a clarification file:

```markdown
# Requirements Clarification

I detected contradictions in your responses:

## Contradiction 1: Scope vs Impact
You indicated "bug fix" (Q1: A) but also "affects multiple services" (Q3: C).
These seem contradictory.

### Clarification Question 1
Which better describes this work?

A) Small bug fix isolated to one service
B) Bug fix that requires coordinated changes across services
C) Actually a feature enhancement, not a bug fix
D) Other (please describe after [Answer]: tag below)

[Answer]: 
```

**Do NOT proceed until contradictions are resolved.**

---

## Error Handling

### Missing Answers

```
Question [X] is not answered. Please provide a letter choice before proceeding.
```

### Invalid Answers

```
Question [X] has invalid answer '[answer]'. Please use a letter choice (A, B, C, etc.).
```

### Ambiguous Answers

```
For Question [X], please provide the letter choice. If none match, choose 'Other'
and add your description after the [Answer]: tag.
```

---

## Summary

| Do | Don't |
|----|-------|
| ✅ Create question files | ❌ Ask questions in chat |
| ✅ Use [Answer]: tags | ❌ Expect inline responses |
| ✅ Include "Other" option | ❌ Force limited choices |
| ✅ Check for contradictions | ❌ Proceed with conflicts |
| ✅ Wait for completion | ❌ Assume answers |
