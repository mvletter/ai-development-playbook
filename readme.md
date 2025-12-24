# AI Development Workflow Templates

Templates for AI-assisted development with Claude Code.

## Structure

```
ai-development-setup/
├── ai-workflow-spec.md      # Philosophy and workflow
├── CLAUDE.md                # Always-loaded context
├── claude-progress.txt      # Cross-session progress tracking
├── project.md               # Feature index
│
├── prompts/                 # Pre-project architecture discovery
│   ├── discovery.md         # Phase 1: Understand the problem
│   ├── architecture-pattern.md  # Phase 2: Choose structure
│   ├── tech-stack.md        # Phase 3: Pick technologies
│   └── validator.md         # Phase 4: Stress-test decisions
│
├── .claude/
│   ├── commands/            # Slash commands
│   │   ├── setup.md         # Run prompts → initialize project
│   │   ├── research.md
│   │   ├── plan.md
│   │   ├── implement.md
│   │   ├── wrap.md          # Session-end analysis → inbox.md
│   │   ├── retro.md         # Bug analysis → pitfalls.md
│   │   ├── complete.md      # Cleanup + refactor + review + docs
│   │   ├── cleanup.md
│   │   ├── refactor.md
│   │   ├── security.md
│   │   └── evolve.md        # Analyze and improve agents
│   └── agents/              # Subagents
│       ├── code-reviewer.md     # Check code vs pitfalls/patterns
│       ├── doc-sync-checker.md  # Verify docs match code
│       ├── debugger.md          # Bug analysis and root cause tracing
│       └── performance-optimizer.md  # Bottleneck identification
│
└── docs/
    ├── architecture.md
    ├── database.md
    ├── decisions.md
    ├── inbox.md             # Unprocessed bugs & ideas
    ├── pitfalls.md          # Single file → splits when large
    ├── patterns.md          # Single file → splits when large
    └── features/            # [name].md for features, [type]-[date].md for maintenance
        ├── _template.md
        └── done/
```

## New project setup

### Option A: Use /setup command (recommended)

```
/setup
```

Guides you through 4 phases (30-60 min):
1. **Discovery** - Understand the problem, validate build vs buy
2. **Architecture** - Choose pattern (monolith, services, serverless)
3. **Tech Stack** - Pick specific technologies
4. **Validation** - Stress-test decisions, get approval

Result: Complete project documentation, ready to build.

### Option B: Run prompts manually

Use the prompts in `prompts/` sequentially:

```
prompts/discovery.md         → Requirements Summary
prompts/architecture-pattern.md → Architecture ADR
prompts/tech-stack.md        → Tech Stack ADR
prompts/validator.md         → Validation Report
```

Each phase produces a document that feeds the next.

### After setup

1. Copy templates to your project
2. Fill in project-specific content in `CLAUDE.md` (from /setup output)

## Workflow phases

```
/research [idea]     → Human-readable problem + validation → docs/features/[name].md
/plan [feature]      → Solution design + tasks breakdown
/implement [feature] → Per-task: test → code → merge → progress
/wrap                → Session-end analysis → catches missed bugs/ideas → docs/inbox.md
/retro [bug]         → Analyze critical bug, prevent recurrence → docs/pitfalls.md
/complete [feature]  → Auto-generate summary + wrap check → docs/features/done/
```

## Quick capture (during any session)

Say these words to capture items instantly:

| Trigger | Action |
|---------|--------|
| `bug` | Log to docs/inbox.md |
| `idea` | Log to docs/inbox.md |

Fixed bugs are auto-detected by `/wrap` at session end.

## Key concepts

**Layered context**: CLAUDE.md always loaded (<500 lines), docs loaded on-demand

**Architecture-first**: Always read docs/architecture.md at feature start to understand where code belongs

**60% rule**: Never exceed 60% context window. Commit, update progress, start fresh.

**Clean sessions**: Every session ends with mergeable code and updated progress

**Progress tracking**: claude-progress.txt for cross-session continuity

**Code-to-docs**: AI-CONTEXT comments point to patterns/pitfalls

**Component constraints**: architecture.md defines what each component can/cannot do

**Category-prefixed anchors**: `#security-sql-injection`, `#api-response-format`
- Enables splitting files later without changing code references

**Self-evolving docs**:
- pitfalls.md: Splits at >50 entries or >15k tokens
- patterns.md: Splits at >30 patterns or >10k tokens
- architecture.md: Splits if >1000 tokens (into architecture/ subdirectory)
- Old file becomes index, code references stay the same

## Advanced features

**Subagents**: For complex investigation without filling main context
- `code-reviewer` - Checks code against pitfalls.md and patterns.md (auto-runs in /complete)
- `doc-sync-checker` - Verifies docs match code (auto-runs in /complete)
- `debugger` - Analyzes bugs, traces root causes, proposes fixes
- `performance-optimizer` - Identifies bottlenecks, suggests optimizations
- Agents learn automatically as pitfalls.md and patterns.md grow
- Run `/evolve` periodically to evaluate if new agents are needed

**Hooks**: Automated linting, testing, formatting via .claude/settings.json (generated by /setup)

**Modular rules**: .claude/rules/ directory for organized instructions

**Think modes**: "think hard", "ultrathink" for complex decisions
