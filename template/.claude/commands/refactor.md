# /refactor

Analyze code quality and generate a feature spec for improvements.

## When to use

| Situation | Command |
|-----------|---------|
| Standalone session (bugs, maintenance) | `/refactor` standalone |
| Feature development | Via `/complete` (Phase 3) |
| Periodic quality improvement | `/refactor` standalone |

## Instructions

Read these docs:
- docs/architecture.md (intended patterns)
  - If split: also read docs/architecture/components.md
- docs/patterns.md (established solutions)
- docs/decisions.md (don't reverse settled decisions)

## Analysis protocol (20 minutes)

### Code smell detection

**Function issues:**
- Functions > 20 lines
- Functions with multiple responsibilities
- Deep nesting (> 3 levels)
- Too many parameters (> 4)

**Duplication:**
- Similar code in multiple places
- Copy-pasted logic with minor variations
- Patterns that should be extracted

**Coupling:**
- Business logic mixed with infrastructure
- Hard-coded dependencies
- Tight coupling between unrelated modules

**Consistency:**
- Different error handling approaches
- Inconsistent logging patterns
- Mixed architectural styles

### Prioritize

**High impact, low risk**: Do first
**High impact, high risk**: Break into smaller steps
**Low impact, any risk**: Consider skipping

## Output

Create `docs/features/refactor-[YYYY-MM-DD].md` using feature template.

Include:
- Top 3 issues with specific examples
- Tasks for each refactor
- Reference to docs/patterns.md for target patterns

Update project.md with link under TODO.
Update claude-progress.txt with refactor status.

## Do NOT

- Change behavior (refactor preserves behavior)
- Add features
- Fix bugs (unless the bug IS the code smell)
- Refactor without existing test coverage
- Reverse decisions in docs/decisions.md
