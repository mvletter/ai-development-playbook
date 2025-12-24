# /research [feature idea]

Validate a problem before designing a solution.

## Instructions

Read these docs first:
- docs/architecture.md (understand system + where new code belongs)
  - If split: also read docs/architecture/*.md for component/flow details
- docs/database.md (understand data model)
- docs/decisions.md (don't relitigate settled decisions)

For complex investigation, use subagents:
```
Use a subagent to explore [specific question] and report findings.
```

## Protocol

### Phase 1: Problem validation

For complex or ambiguous problems, use "think hard" to thoroughly analyze.

**Questions to answer:**
- What specific problem are we solving?
- Who experiences this and how often?
- What's the current workaround?
- What evidence exists that this is worth building?

**Red flags to challenge:**
- "We might need this later" → Build when needed
- "It would be nice to have" → Is it worth the cost?
- "Other systems do this" → Does OUR system need it?

### Phase 2: Evidence gathering

Collect concrete evidence:
- User feedback or complaints
- Error logs or metrics
- Support tickets
- Time spent on workarounds

No evidence = no validated problem. Stop here and discuss.

### Phase 3: Dependency mapping

**Identify:**
- Which existing files would be affected?
- What other features depend on these areas?
- What could break if changes go wrong?

### Phase 4: Initial security check

**Consider:**
- What sensitive data might this involve?
- What authentication/authorization is implied?
- Are there obvious security concerns?

## Output

Create `docs/features/[name].md` using the template with:
- **"What we're solving"** - Plain language problem description for non-coders (human-readable context)
- Problem section (what, who, evidence, current workaround)
- Dependencies section (affected areas, risks)
- Security considerations (initial assessment)
- Status: Research (not Ready for Planning)

**Example "What we're solving":**
> "Sarah found someone logged into the admin panel who shouldn't have access. We need to add login to protect the admin panel."

Update `project.md` with link to new feature spec under TODO.
Update `claude-progress.txt` with research status.

## Ready for /plan

Proceed to /plan when ALL of these are true:
- Problem is validated with concrete evidence
- Clear "who experiences this" answer exists
- Dependencies are mapped and manageable
- Security considerations are identified

Then proceed to `/plan [feature-name]`.

---

## Stop conditions

**Do NOT proceed to /plan if:**
- Problem is not validated with concrete evidence
- Problem is vague ("improve performance" without specifics)
- No clear "who experiences this" answer
- Dependencies are too complex (break into smaller problems)

**Instead:** Document findings and discuss with user.

## Do NOT

- Design solutions (that's /plan)
- Break into tasks (that's /plan)
- Start implementation
- Assume the problem is valid without evidence
