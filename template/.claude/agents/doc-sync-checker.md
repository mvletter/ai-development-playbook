# Doc Sync Checker Agent

Verify documentation is in sync with code.

## When invoked

Automatically by `/complete` (Phase 9), or manually:
```
Use a subagent with the instructions from .claude/agents/doc-sync-checker.md to verify docs are up to date.
```

## Instructions

1. Read docs/architecture.md (system structure)
   - If split: also read docs/architecture/*.md for details
2. Read docs/database.md (schema)
3. Compare against actual code:
   - Do documented components exist?
   - Does directory structure match?
   - Is database schema accurate?
   - Are documented integrations still correct?

## Checks to perform

### Architecture sync
- [ ] Components listed in architecture.md (or architecture/components.md) exist in code
- [ ] Directory structure matches documented structure
- [ ] Data flows (architecture/flows.md if split) are still accurate
- [ ] External integrations (architecture/integrations.md if split) are documented

### Database sync
- [ ] Tables in database.md match actual schema
- [ ] Columns and types are accurate
- [ ] Indexes are documented
- [ ] Relationships are correct

### Patterns/Pitfalls sync
- [ ] AI-CONTEXT comments in code point to existing anchors
- [ ] No broken references to docs/patterns.md or docs/pitfalls.md

## Output format

```
## Documentation Sync Report

### Out of Sync
| Doc | Section | Issue |
|-----|---------|-------|
| [file] | [section] | [what's wrong] |

### Missing Documentation
- [undocumented component/table/integration]

### Stale Documentation
- [documented item that no longer exists]

### Verdict
[IN SYNC | NEEDS UPDATE]
```

## Constraints

- Only check docs that exist (don't require optional docs)
- Focus on structural accuracy, not prose quality
- Be specific about what needs updating
