# /plan [feature-name]

Design the solution and break into implementable tasks.

> **Note:** `$ARGUMENTS` = the name you provide when running this command.
> Example: `/plan user-auth` → `$ARGUMENTS` becomes `user-auth`

## Prerequisites

Feature must have a validated Problem section in docs/features/[name].md (created by /research).

## Instructions

Read these docs:
- docs/features/$ARGUMENTS.md (the research output - required)
- docs/architecture.md (system constraints + where code belongs)
  - If split: also read docs/architecture/components.md for component details
- docs/patterns.md (existing solutions to reuse)
- docs/decisions.md (don't contradict settled decisions)

## Protocol

### Phase 1: Solution design

Think hard about the approach before committing.

**Questions to answer:**
- What's the simplest solution that solves the validated problem?
- Can we extend existing features instead of building new?
- What would "good enough" look like vs "perfect"?

**Document alternatives:**
- Option A: [approach] - [pros/cons]
- Option B: [approach] - [pros/cons]
- Option C: Do nothing - [what happens]

Choose one. Document why others were rejected.

### Phase 2: Scope definition

**Must have** (essential for solving the problem):
- [ ] [requirement]
- [ ] [requirement]

**Won't have** (explicit exclusions):
- [Feature X - out of scope]
- [Feature Y - not needed for this problem]

Be aggressive with "Won't have". AI will try to expand scope during implementation.

### Phase 3: Test scenarios

Define test scenarios in plain language BEFORE task breakdown. This is "Specification by Example".

**Format:**
```markdown
## Test Scenarios

### Happy path
- [User does X] → [Expected result]
- [User does Y] → [Expected result]

### Edge cases
- [Empty input] → [What should happen]
- [Maximum values] → [What should happen]

### Error cases
- [Invalid input] → [Expected error message/behavior]
- [System failure] → [Graceful degradation]
```

**Rules:**
- Plain language (no code)
- Concrete examples with real values
- Each scenario = one test
- Cover: success, boundaries, failures

**Ask user:** "Are these the right scenarios? Anything missing?"

Wait for approval before proceeding to Phase 4.

### Phase 4: Task breakdown

Each task must:
- Be completable in one branch
- Touch specific, listed files
- Have no file overlap with other tasks
- Include a clear test/validation
- End with mergeable code

**Task format:**
```markdown
### T1: [Name]
- **Files**: [specific files this task modifies]
- **Depends**: [other task IDs, or "none"]
- **Test**: [how to verify this works]
- **Done**: [acceptance criterion]
```

### Phase 5: Boundaries check

Before finishing, verify:
- [ ] No parallel tasks touch same files
- [ ] Each task is < 2 hours of work
- [ ] Each task results in mergeable code
- [ ] Interfaces between tasks are defined
- [ ] Rollback strategy is clear

## Output

Update `docs/features/$ARGUMENTS.md` with:
- Solution section (chosen approach + rejected alternatives)
- Scope section (must have + won't have)
- Tasks section (numbered, with file boundaries)
- Rollback section
- Status: Ready

Update `project.md`: move feature from TODO to Active.
Update `claude-progress.txt` with planning status.

## Stop conditions

**Do NOT proceed to implementation if:**
- Tasks have file overlap (rework boundaries)
- Any task is too large (break it down further)
- Scope is unclear (define what's NOT included)
- No rollback strategy exists

## Ready for /implement

Proceed to /implement when ALL of these are true:
- Solution is chosen with alternatives documented
- Scope has explicit "Must have" and "Won't have"
- Test scenarios are approved by user
- All tasks have file boundaries (no overlap)
- Each task links to specific test scenarios
- Rollback strategy is defined

Then proceed to `/implement [feature-name]`.

## Do NOT

- Skip the alternatives analysis
- Create tasks without specific file lists
- Allow scope creep ("while we're at it...")
- Start implementation in this phase
