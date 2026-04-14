---
name: sync-upstream
description: Review and incorporate upstream AIDLC changes. Use when there's an upstream sync issue or when user says "sync upstream", "check AIDLC updates", or "upstream changes".
---

# Sync Upstream Skill

Review upstream AWS AIDLC changes and incorporate them meaningfully into this toolkit.

## When to Use

- An upstream sync issue was created by the GitHub Action
- User wants to check for AIDLC methodology updates
- Incorporating new best practices from AWS

## Workflow

```
1. Fetch upstream changes
2. Analyze relevance to our toolkit
3. Use /brainstorming to design incorporation
4. Create PR with adapted changes
```

---

## Step 1: Fetch Upstream Changes

```bash
# Clone or update upstream
git clone --depth=50 https://github.com/awslabs/aidlc-workflows.git /tmp/upstream-aidlc

# Or if already cloned
cd /tmp/upstream-aidlc && git pull
```

If an issue exists, read the issue for the commit range.

---

## Step 2: Analyze Changes

For each changed file, determine:

| Question | If Yes |
|----------|--------|
| Is this a core methodology change? | High priority - must review |
| Is this a new rule/extension? | Consider adding |
| Is this a template update? | Adapt to our format |
| Is this documentation only? | Update if relevant |
| Is this workshop-specific? | Skip (we removed workshop focus) |

### Key Directories to Watch

| Upstream Path | Our Equivalent | Action |
|---------------|----------------|--------|
| `.aidlc-rule-details/common/` | `.aidlc-rule-details/common/` | Review for methodology changes |
| `.aidlc-rule-details/extensions/` | `.aidlc-rule-details/extensions/` | Consider new security rules |
| `templates/` | `templates/` | Adapt to our streamlined format |

---

## Step 3: Design Incorporation

For non-trivial changes, use brainstorming:

```
/brainstorming

I need to incorporate upstream AIDLC changes into our toolkit.

Changes detected:
- [List key changes from issue/analysis]

Our toolkit differences:
- Removed workshop structure
- Added superpowers integration
- Token-efficient skill structure
- Streamlined for solo/small team use

Help me decide what to incorporate and how to adapt it.
```

---

## Step 4: Create PR

### For Simple Updates

Direct adaptation:

```bash
# Create branch
git checkout -b sync/upstream-YYYY-MM-DD

# Copy/adapt files
# ... make changes ...

# Commit with context
git commit -m "sync: incorporate upstream AIDLC changes

Changes from awslabs/aidlc-workflows:
- [Change 1]
- [Change 2]

Adapted for our toolkit:
- [Adaptation 1]
- [Adaptation 2]

Upstream commit: [sha]"
```

### For Complex Updates

Use `/writing-plans` to plan the incorporation:

```
/writing-plans

Based on the brainstorming session, create a plan to incorporate
the upstream AIDLC changes.
```

---

## Decision Guide

### Always Incorporate

- Security rule updates
- Core methodology improvements
- Bug fixes in templates

### Adapt Before Incorporating

- Workshop-specific content → Remove workshop framing
- Verbose templates → Streamline for our format
- New phases/stages → Evaluate fit with superpowers workflow

### Skip

- Workshop facilitation content
- Multi-team coordination (unless useful)
- Redundant with superpowers skills

---

## PR Template

```markdown
## Sync Upstream AIDLC Changes

### Source
- Upstream repo: awslabs/aidlc-workflows
- Commit range: [from]...[to]
- Issue: #XXX

### Changes Incorporated

| Upstream Change | Our Adaptation |
|-----------------|----------------|
| [Change] | [How we adapted it] |

### Changes Skipped

| Upstream Change | Reason |
|-----------------|--------|
| [Change] | [Why we skipped it] |

### Testing

- [ ] Skills still work correctly
- [ ] Templates are valid
- [ ] Documentation is consistent

### Checklist

- [ ] Used /brainstorming for non-trivial changes
- [ ] Maintained superpowers integration
- [ ] Kept token-efficient structure
- [ ] Updated GETTING-STARTED.md if needed
```

---

## Quick Reference

```bash
# Check upstream manually
gh api repos/awslabs/aidlc-workflows/commits/main --jq '.sha'

# Compare changes
open "https://github.com/awslabs/aidlc-workflows/compare/OLD_SHA...NEW_SHA"

# Trigger workflow manually
gh workflow run check-upstream.yml
```
