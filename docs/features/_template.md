# Feature: [Name]

> Status: Research | Planning | Active | Done
> Created: [YYYY-MM-DD]
> Branch: `feature/[name]-`

---
## Human-Readable Context (for non-coders)
---

## What we're solving

[Plain language problem description that anyone can understand]

**Example**: "Sarah found someone logged into the admin panel who shouldn't have access. We need to add login to protect the admin panel."

---
## Filled by /research
---

## Problem

**What**: [Clear problem description]
**Who**: [Affected users]
**Evidence**: [Concrete examples, metrics, user feedback, or support tickets]
**Current workaround**: [How users cope now, or "none"]

## Dependencies

**Affected areas**:
- `[path/area]` - [how it's affected]

**Risks**:
- [What could break]

**Security considerations**:
- [Initial assessment of security implications]

---
## Filled by /plan
---

## Solution

**Approach**: [Chosen solution]

**Alternatives considered**:
- [Option A] - Rejected because: [reason]
- [Option B] - Rejected because: [reason]
- Do nothing - Would result in: [consequence]

**Good enough**: [Minimum viable - what's NOT needed for success]

## Scope

**Must have**:
- [ ] [Essential for launch]
- [ ] [Essential for launch]

**Won't have** (explicit):
- [Feature X - out of scope, maybe later]
- [Feature Y - not needed]

## Test Scenarios

> Plain language, approved by user before implementation.

### Happy path
- [User does X] → [Expected result]
- [User does Y] → [Expected result]

### Edge cases
- [Empty input] → [What should happen]
- [Boundary value] → [What should happen]

### Error cases
- [Invalid input] → [Expected error/behavior]
- [System unavailable] → [Graceful handling]

**Approved**: [ ] Yes / [ ] Needs revision

## Files

**Modify**:
- `[path/file]` - [what changes]

**Create**:
- `[path/file]` - [purpose]

## Tasks

Each task = one branch, no file overlap, ends with mergeable code.

### T1: [Name]
- **Files**: [specific files]
- **Scenarios**: [which test scenarios this implements, e.g., "Happy path 1, Edge case 2"]
- **Done**: [acceptance criterion]

### T2: [Name]
- **Files**: [specific files]
- **Depends**: T1 | parallel
- **Scenarios**: [which test scenarios]
- **Done**: [acceptance criterion]

## Boundaries check

- [ ] No parallel tasks touch same files
- [ ] Each task <2 hours
- [ ] Each task results in mergeable code
- [ ] Interfaces between tasks defined

## Rollback

If this fails: [How to undo - revert commits, feature flag, etc.]

---
## Filled during /implement
---

## Progress

### T1: [Status]
- [ ] Test written
- [ ] Implementation
- [ ] Merged

### T2: [Status]
...

**Related inbox items**: [Bug #X, Idea #Y discovered during this feature, or "none"]

**Session notes**: [Learnings, decisions from current session]

**Blockers**: [Current blockers, or "none"]

---
## Filled at completion
---

## After completion

### Documentation updates
- [ ] docs/architecture.md - [changes, or "none"]
- [ ] docs/database.md - [changes, or "none"]

### New learnings to capture
- [ ] docs/pitfalls.md#[category]-[name] - [new mistake discovered]
- [ ] docs/patterns.md#[category]-[name] - [new reusable solution]

### Code references to add
Add AI-CONTEXT comments pointing to new entries:
```
// AI-CONTEXT: See docs/pitfalls.md#[category]-[name]
// AI-CONTEXT: See docs/patterns.md#[category]-[name]
```

### Technical debt created
- [Shortcuts taken that need future attention, or "none"]

## Completion checklist

- [ ] All tasks merged
- [ ] All tests passing
- [ ] Docs updated
- [ ] AI-CONTEXT comments added
- [ ] claude-progress.txt updated
- [ ] Move to docs/features/done/
- [ ] Update project.md

---
## Filled by /complete (Auto-generated)
---

## What We Built

**Built**: [Start date] - [End date] ([total hours spent])

**How we solved it**:

[2-3 sentences in plain language explaining the solution. Focus on WHAT was built and HOW it works from a user perspective, not technical implementation details.]

**Example**: "Users now register with email and password. The password gets encrypted and stored safely. When they log in, we give them a special token that proves who they are for the next 24 hours."

**Key decisions**:

- [Decision 1 with concrete reasoning - why this choice over alternatives]
- [Decision 2 with concrete reasoning]
- [Decision 3 if applicable]

**Bugs we fixed along the way**:

- [Bug 1: What went wrong, how we discovered it, how we fixed it]
- [Bug 2: What went wrong, how we discovered it, how we fixed it]
- [Or "none" if no bugs encountered]

**What we learned**:

- [Learning 1: Technical insight or process improvement]
- [Learning 2: Things to do differently next time]
- [Learning 3 if applicable]
