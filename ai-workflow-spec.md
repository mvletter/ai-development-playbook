# AI Development Workflow

> Based on: Claude Code Best Practices, Spec-Kit, TDG principles.

## Philosophy

1. **Spec is source of truth** - Living spec in `docs/features/`, not code comments
2. **Continuous integration** - Each task merges to main, not one big integration
3. **Layered context** - Essentials always loaded, detail on-demand
4. **Code points to docs** - AI-CONTEXT comments link to patterns and pitfalls
5. **Self-evolving** - Patterns and pitfalls grow with the project
6. **Clean sessions** - Each session ends with mergeable state

## Structure

```
project/
├── CLAUDE.md                    # Always loaded (<500 lines)
├── project.md                   # Feature index
├── claude-progress.txt          # Cross-session progress log
│
├── .claude/
│   ├── commands/                # Slash commands
│   ├── agents/                  # Subagents (code-reviewer, doc-sync-checker, etc.)
│   └── rules/                   # Modular rules (optional)
│
└── docs/
    ├── architecture.md          # System structure
    ├── database.md              # Schema, conventions
    ├── decisions.md             # ADRs
    ├── inbox.md                 # Unprocessed bugs & ideas
    ├── pitfalls.md              # Mistakes to avoid
    ├── patterns.md              # Copy-paste solutions
    └── features/
        ├── [feature].md         # Active specs
        └── done/                # Completed specs
```

## Workflow phases

```
/research [idea]
→ Validate the problem with evidence
→ Map dependencies and risks
→ Write human-readable problem description (for non-coders)
→ Output: docs/features/[name].md with "What we're solving" + Problem section

/plan [feature]
→ Design solution, consider alternatives
→ Define scope boundaries
→ Break into tasks with file boundaries
→ Use "think hard" for complex decisions
→ Output: Updated spec with Solution, Scope, Tasks

/implement [feature]
→ Per task: branch → test → code → merge
→ Track decisions and bugs as you go
→ Output: Working code on main

/wrap
→ Analyze entire session for missed items
→ Find unlogged bugs, ideas, completed work
→ Add confirmed items to docs/inbox.md
→ Update claude-progress.txt
→ When to use: end of session, before /clear, context filling up

/retro [bug description]
→ Answer 3 questions in plain language (what, why, how to prevent)
→ Determine severity (low/medium/high/critical)
→ Add entry to docs/pitfalls.md
→ Add AI-CONTEXT comment in fixed code
→ When to use: bug took >30min, same bug twice, data loss, security issue

/complete [feature]
→ Auto-generate "What We Built" summary from conversation
→ Capture learnings (patterns, pitfalls)
→ Move spec to done/
→ Update claude-progress.txt
```

## Context management

### The 60% rule

Never exceed 60% context window. Use `/context` to check current usage.

When approaching limit:
1. Update claude-progress.txt with current state
2. Run `/clear` or start new session
3. Resume with fresh context

**Tip:** Configure statusline to show context percentage permanently.

### Layered loading

**Always loaded (CLAUDE.md):**
- Commands, style rules, pitfall triggers
- Keep < 500 lines
- Point to docs: "When working on X, read docs/Y.md"

**On-demand:**
| Doc | Load when |
|-----|-----------|
| docs/features/[x].md | Working on that feature |
| docs/pitfalls.md | During implementation |
| docs/patterns.md | Building something new |
| docs/architecture.md | New feature, major change |

### Explicit file reading

**Important:** @imports load files immediately at session start, not on-demand. To keep context small, don't use @imports. Instead, instruct Claude when to read files:

```markdown
# Documentation
When working on database code: Read docs/database.md
When implementing new features: Read docs/patterns.md
When stuck or before commit: Read docs/pitfalls.md
When starting a feature: Read the feature spec in docs/features/
```

This keeps CLAUDE.md small and loads context only when needed.

---

## Session management

### Clean session endings

Every session should end with:
1. Code in mergeable state (no half-implementations)
2. Tests passing
3. Progress logged in claude-progress.txt
4. Clear handover notes for next session

### Progress tracking

Create `claude-progress.txt` at project root as a **router** (points to source of truth, doesn't duplicate):

```markdown
# Progress Router

Cross-session tracking. Points to source of truth, doesn't duplicate it.

## Active context

Type: [Feature | Maintenance | Bug | none]
Source: [docs/features/name.md | inline]
Last session: [YYYY-MM-DD]

## Quick resume

[3-5 sentences: where we left off, what to do next]

## Current work (inline only)

Use this section ONLY when Type is Maintenance or Bug (no feature spec).
When work grows complex, promote to feature spec.

- [ ] [task]
- [ ] [task]

## Session history

### [YYYY-MM-DD]
- Type: [Feature/Maintenance/Bug]
- Context: [feature name or description]
- Completed: [what got done]
- Handover: [what's next]
```

### Session handover

At end of session (or when context fills):
1. Commit working code
2. Update claude-progress.txt
3. Add "Notes for next session"
4. Start fresh with `/clear`

New session reads claude-progress.txt first.

---

## Task execution

For each task:

1. **Branch** - `git checkout -b feature/[name]-[task]`
2. **Test first** - Write failing test (see TDD below)
3. **Implement** - Minimal code to pass
4. **Verify** - Check pitfalls, lint, run tests
5. **Merge** - To main, delete branch
6. **Update** - Spec progress + claude-progress.txt

### TDD workflow (critical)

Test-driven development is Anthropic's "favorite workflow" for agentic coding. AI needs clear, verifiable targets. Without tests first, AI writes tests that validate what the code does, not what it should do.

**Critical: Explicitly state you're doing TDD.** This prevents AI from creating mock implementations.

```
1. "Write a failing test for [feature]" → AI writes test
2. Run test, confirm it fails (red)
3. Commit the failing test
4. "Now implement to make the test pass" → AI writes implementation
5. Run test, confirm it passes (green)
6. Commit the implementation
```

Document your TDD approach in CLAUDE.md so every session follows it.

## Checkpoints (every 6 actions)

1. Still solving right problem?
2. Code matches pitfall patterns?
3. Main still working?
4. Scope creeping?
5. Context window filling up?

---

## What to save where

After each session, capture learnings:

| Discovered | Save to | Format |
|------------|---------|--------|
| Bug (not yet fixed) | docs/inbox.md | Add to Bugs table |
| Idea (not yet planned) | docs/inbox.md | Add to Ideas table |
| Bug caused by pattern | docs/pitfalls.md | `#[category]-[name]` anchor |
| Reusable solution | docs/patterns.md | `#[category]-[name]` anchor |
| Architecture change | docs/architecture.md | Update relevant section |
| Schema change | docs/database.md | Update schema + migration |
| Design decision | docs/decisions.md | New ADR entry |
| Technical debt | Feature spec | "Technical debt" section |
| Session progress | claude-progress.txt | Update log |

**Quick capture:** Say `bug` or `idea` during session → logs to inbox.md
**Session end:** Run `/wrap` → catches missed items

**Always also:**
- Add AI-CONTEXT comment in code pointing to doc
- Update CLAUDE.md if critical pitfall trigger

---

## Code-to-docs linking

Use `AI-CONTEXT` comments to connect code to docs:

```
// AI-CONTEXT: See docs/pitfalls.md#async-unhandled-promise
// AI-CONTEXT: See docs/patterns.md#api-response-format
// AI-CONTEXT: See docs/decisions.md#adr-003
```

**When to add:**
- Code that previously caused bugs → link to pitfall
- Code using established pattern → link to pattern
- Non-obvious design choice → link to ADR

---

## Patterns and pitfalls

### Anchor format

Use category-prefixed anchors from day one:

```
#async-unhandled-promise
#security-sql-injection
#api-response-format
#data-transaction
```

This enables splitting later without changing code references.

### Categories

**Pitfalls:**
- async - Promises, timing, race conditions
- security - Auth, validation, injection
- database - Queries, transactions, migrations
- api - HTTP, responses, error handling
- state - Caching, consistency, side effects

**Patterns:**
- api - Endpoints, responses, validation
- data - Database access, queries, transactions
- error - Catching, throwing, logging
- auth - Authentication, authorization
- test - Test patterns, mocks, fixtures

### Scaling

**Pitfalls** - Split at >50 entries OR >15k tokens
**Patterns** - Split at >30 patterns OR >10k tokens

After split, old file becomes index. Code references don't change.

---

## Subagents

Subagents are specialized AI assistants with their own context window. They prevent context pollution in your main thread.

**Built-in:** `general-purpose` (always available)

**Included with this workflow:**

| Agent | Purpose | Invoked by |
|-------|---------|------------|
| code-reviewer | Check code against pitfalls.md and patterns.md | /complete (Phase 4) |
| doc-sync-checker | Verify docs match code | /complete (Phase 9) |
| debugger | Analyze bugs, trace root causes, propose fixes | Manual |
| performance-optimizer | Identify bottlenecks, suggest optimizations | Manual |

**Location:** `.claude/agents/`

**When to use manually:**
- Deep codebase exploration
- Researching external APIs/libraries
- Investigating bugs with many possible causes
- Gathering context for a decision

**How to invoke:**
```
Use a subagent to research [topic] and report back findings.
Use a subagent with the instructions from .claude/agents/code-reviewer.md to review my changes.
Use a subagent with the instructions from .claude/agents/doc-sync-checker.md to verify docs are up to date.
```

Subagent does the heavy lifting, returns summary to main context.

**Self-improving agents:**

Agents read pitfalls.md and patterns.md dynamically, so they learn as those files grow.

| Trigger | What happens |
|---------|--------------|
| /retro adds pitfall | Agent relevance check: which agent should know? |
| /complete finishes | Agent learning check: did agents miss anything? |
| /evolve (manual) | Full analysis: clusters, gaps, new agent candidates |

Run `/evolve` periodically (e.g., after 3-5 features) to evaluate if agents need improvement.

---

## Hooks (optional automation)

Claude Code hooks automate quality checks. Add to `.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write \"$FILE\""
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash.*git commit",
        "hooks": [
          {
            "type": "command",
            "command": "npm test"
          }
        ]
      }
    ]
  }
}
```

**Adapt for your stack:**
- JavaScript: `npx prettier --write` / `npm test`
- Python: `black "$FILE"` / `pytest`
- Go: `gofmt -w "$FILE"` / `go test ./...`

**Hook types:**
- `PostToolUse`: Runs after file edits (formatting, linting)
- `PreToolUse`: Runs before actions (test before commit)
- `Notification`: Show alerts

---

## Modular rules (optional)

For larger projects, use `.claude/rules/` instead of one large CLAUDE.md:

```
.claude/
├── CLAUDE.md           # Core instructions
└── rules/
    ├── code-style.md   # Style guidelines
    ├── testing.md      # Test conventions
    └── security.md     # Security requirements
```

All `.md` files in rules/ are automatically loaded.

---

## Parallel agents

When multiple agents work:

- Spec defines tasks with file boundaries
- No file overlap between parallel tasks
- Interfaces defined upfront
- Each agent merges independently
- Conflict = stop, reassess boundaries

---

## Think modes

For complex decisions, use extended thinking:

- `"think"` - Standard analysis
- `"think hard"` - Deeper consideration
- `"think harder"` - Complex tradeoffs
- `"ultrathink"` - Major architectural decisions

Example: "Think hard about how to structure the authentication flow."
