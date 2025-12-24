# /evolve

Analyze the workflow and suggest agent improvements.

## When to use

- After completing several features
- When you notice agents missing things
- Periodically (e.g., monthly) to keep agents effective
- NOT part of standard workflow - run when needed

## Instructions

Read these docs:
- docs/pitfalls.md (all entries)
- docs/patterns.md (all entries)
- docs/decisions.md (architecture rules)
- docs/architecture.md (component constraints)
- docs/features/done/ (completed features)
- .claude/agents/ (current agents)

## Analysis protocol

Use "think hard" for this analysis - it requires seeing patterns across multiple sources.

### 1. Cluster analysis

Group pitfalls and patterns by category prefix:
- How many entries per category?
- Are there clusters without a dedicated agent?

**Threshold:** 3+ related entries in a category without agent coverage → suggest new agent

### 2. Agent effectiveness

For each existing agent, check:
- Does it cover all relevant pitfalls?
- Does it check all relevant patterns?
- Were there issues in recent features that this agent should have caught?

**Gaps found:** Suggest specific additions to agent checklist

### 3. New agent candidates

Based on:
- Pitfall clusters
- Pattern clusters
- Completed features (domain knowledge)
- Tech stack (framework-specific)
- Integrations (API-specific)

### 4. Agent consolidation

Are there agents with overlapping concerns?
- Suggest merging if overlap > 50%

## Output

```
## Agent Evolution Report

### Current agents
| Agent | Covers | Last relevant update |
|-------|--------|---------------------|
| code-reviewer | pitfalls, patterns | [date of newest pitfall] |
| doc-sync-checker | architecture, database | [date] |

### Suggested improvements

**Existing agents:**
- [agent]: Add check for [specific pitfall/pattern]
- [agent]: Now covers [X] entries (up from [Y])

**New agents to consider:**
| Candidate | Reason | Entries it would cover |
|-----------|--------|----------------------|
| [name] | [why needed] | [count] |

**No action needed:**
- [category]: Only [X] entries, not enough for dedicated agent

### Recommendation
[EVOLVE | MAINTAIN]

If EVOLVE: prioritized list of changes
If MAINTAIN: system is balanced, check again after [X] more features
```

## Do NOT

- Create agents for every small cluster
- Suggest agents without clear purpose
- Overcomplicate the agent system
- Force evolution when not needed
