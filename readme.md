# AI Development Playbook

A structured approach to building software with AI assistance.

**What is this?** A set of instructions and templates that help you and an AI assistant (like Claude) work together to build software - from initial idea to finished product.

## Getting the template

### Option 1: Download ZIP (simplest)

1. Go to [Releases](https://github.com/mvletter/ai-development-playbook/releases)
2. Download the latest `template.zip`
3. Extract to your project folder

### Option 2: Git sparse checkout (for git users)

Only clone the `template/` folder:

```bash
git clone --filter=blob:none --sparse https://github.com/mvletter/ai-development-playbook.git
cd ai-development-playbook
git sparse-checkout set template
cp -r template/* /path/to/your/project/
```

### Option 3: Full clone + copy

```bash
git clone https://github.com/mvletter/ai-development-playbook.git
cp -r ai-development-playbook/template/* /path/to/your/project/
```

## Repository structure

```
ai-development-playbook/
├── readme.md                # This file (about the playbook)
├── version                  # Current playbook version
├── changelog.md             # What's new in each version
│
└── template/                # ← COPY THIS TO YOUR PROJECT
    ├── .playbook-version    # Tracks which version you copied
    ├── CLAUDE.md            # Always-loaded context
    ├── claude-progress.txt  # Cross-session progress tracking
    ├── project.md           # Feature index
    │
    ├── prompts/             # Pre-project architecture discovery
    │   ├── discovery.md
    │   ├── architecture-pattern.md
    │   ├── tech-stack.md
    │   └── validator.md
    │
    ├── .claude/
    │   ├── commands/        # Slash commands (/setup, /plan, /implement, etc.)
    │   └── agents/          # Subagents (debugger, code-reviewer, etc.)
    │
    └── docs/
        ├── system.md        # What it is, problem, users, capabilities
        ├── architecture.md  # System overview (splits to architecture/ when large)
        ├── database.md
        ├── decisions.md
        ├── inbox.md         # Unprocessed bugs & ideas
        ├── pitfalls.md
        ├── patterns.md
        └── features/
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

Fill in project-specific content in `CLAUDE.md` (from /setup output)

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
- architecture.md: Splits at ~300 lines into architecture/ subdirectory:
  - `architecture.md` → index (diagram, structure, decisions, links)
  - `architecture/components.md` → component details, constraints
  - `architecture/flows.md` → data flows step-by-step
  - `architecture/integrations.md` → external APIs, boundaries, security
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

## Staying up to date

The playbook evolves as we learn. To check for updates:

```
/playbook-update
```

This will:
1. Check if there's a newer version (compares your `.playbook-version` with latest)
2. Show what's changed (in plain language)
3. Give you instructions on how to apply relevant updates

**Important:** Updates are never automatic. You read the changelog and decide what applies to your project. Your customizations are never overwritten.

You can also check manually: [changelog.md](https://github.com/mvletter/ai-development-playbook/blob/master/changelog.md)
