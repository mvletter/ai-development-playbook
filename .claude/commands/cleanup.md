# /cleanup

Analyze codebase for hygiene issues and generate a feature spec for fixes.

## When to use

| Situation | Command |
|-----------|---------|
| Standalone session (bugs, maintenance) | `/cleanup` standalone |
| Feature development | Via `/complete` (Phase 2) |
| Periodic codebase cleanup | `/cleanup` standalone |

## Instructions

Read docs/architecture.md to understand intended structure.

## Analysis protocol (15 minutes)

### Scan for issues

**Unused code:**
- Files not imported anywhere
- Functions not called
- Dead code paths
- Commented-out code blocks

**Inconsistencies:**
- Naming convention violations
- Pattern inconsistencies across similar files
- Mixed approaches to same problem

**Misplaced files:**
- Test files outside tests/
- Config in wrong location
- Temporary files committed

**Dependency issues:**
- Unused packages
- Outdated dependencies with security issues
- Duplicate dependencies

### Validate findings

Before flagging as "unused":
- Check dynamic imports
- Check test files
- Check build scripts
- Verify not referenced in config

### Prioritize

**High**: Security issues, broken code, blocking issues
**Medium**: Consistency, maintainability
**Low**: Style, minor optimization

## Output

Create `docs/features/cleanup-[YYYY-MM-DD].md` using feature template.

Include:
- Top 3 issues with impact
- Tasks ordered by priority
- Tests verifying no regression

Update project.md with link under TODO.
Update claude-progress.txt with cleanup status.

## Do NOT

- Delete files without verifying they're unused
- Refactor logic (that's /refactor)
- Fix bugs (that's a feature)
- Make "improvements" beyond hygiene
