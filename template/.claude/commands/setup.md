# /setup

Initialize a new project with proper architecture discovery.

## When to use

At the START of a new project, before writing any code.

## Overview

This command guides you through 4 phases that mirror how experienced architects work:

| Phase | Prompt | Output |
|-------|--------|--------|
| 1. Discovery | prompts/discovery.md | Requirements Summary |
| 2. Architecture | prompts/architecture-pattern.md | Architecture Decision Record |
| 3. Tech Stack | prompts/tech-stack.md | Tech Stack ADR |
| 4. Validation | prompts/validator.md | Validation Report + Architecture Doc |

**Time:** 30-60 minutes total
**Result:** Complete project documentation, ready to build

## Instructions

Guide the user through each phase sequentially. Each phase produces a document that feeds the next.

---

### Phase 1: Discovery

Read and execute `prompts/discovery.md`.

**Goal:** Understand the problem before solving it.

**Process:**
1. Conduct conversational interview
2. Ask follow-up questions (max 2 at a time)
3. Challenge assumptions ("have you looked at existing solutions?")
4. Cover: problem, users, scale, team, timeline, security

**Output:** Requirements Summary

**Do not proceed until:**
- Problem is validated with concrete evidence
- Build vs buy is justified
- Team capabilities are known
- Timeline is understood

---

### Phase 2: Architecture Pattern

Read and execute `prompts/architecture-pattern.md`.

**Input:** Requirements Summary from Phase 1

**Goal:** Choose the right structural approach.

**Process:**
1. Analyze requirements
2. Compare 2-4 viable patterns (monolith, services, serverless, etc.)
3. Evaluate: team fit, complexity, time to v1, scaling

**Output:** Architecture Decision Record (ADR)

**Do not proceed until:**
- Pattern is chosen with clear rationale
- Tradeoffs are explicit
- Risks are identified

---

### Phase 3: Tech Stack

Read and execute `prompts/tech-stack.md`.

**Input:** Requirements Summary + Architecture ADR

**Goal:** Pick specific technologies that fit.

**Process:**
1. For each layer (frontend, backend, database, infrastructure)
2. Compare 2-3 options per layer
3. Evaluate: team skills, ecosystem, ops complexity, AI support

**Output:** Tech Stack ADR

**Do not proceed until:**
- All layers have technology choices
- Stack coherence is verified
- Cost estimate exists

---

### Phase 4: Validation

Read and execute `prompts/validator.md`.

**Input:** All three previous documents

**Goal:** Stress-test the decisions before committing.

**Process:**
1. Check consistency across documents
2. Identify gaps and blind spots
3. Run failure scenarios
4. Produce verdict: APPROVED / APPROVED WITH CONCERNS / BLOCKED

**Output:**
- Validation Report → show to user (not saved as file)
- Architecture Document → use as source for `docs/architecture.md` (see Documentation section)

---

## Handling verdicts

| Verdict | Action |
|---------|--------|
| **APPROVED** | Proceed to documentation (below) |
| **APPROVED WITH CONCERNS** | Address concerns first, then proceed |
| **BLOCKED** | Do NOT proceed. See recovery below. |

### Recovery from BLOCKED

If validator returns BLOCKED:

1. **Read the blocking issues** - Validator explains why
2. **Go back to the relevant phase:**
   - Requirements unclear? → Restart Phase 1 (Discovery)
   - Architecture mismatch? → Restart Phase 2 (Architecture)
   - Tech stack problems? → Restart Phase 3 (Tech Stack)
3. **Re-run Phase 4** after fixing
4. **Do NOT skip validation** - BLOCKED means real problems exist

---

## After validation is APPROVED

### Phase 5: Project Initialization

Before creating documentation, initialize the development environment:

**1. Git Repository**
```bash
git init
git add .
git commit -m "Initial project structure"
```

**2. Generate .gitignore**

Based on Tech Stack ADR, create appropriate .gitignore:

1. Look up standard .gitignore for the chosen language/framework
2. Add patterns for:
   - Build artifacts and compiled output
   - Dependencies (vendor folders, package caches)
   - Environment files (.env, *.local)
   - IDE/editor files
   - OS files (.DS_Store, Thumbs.db)
   - Log files

**3. Test Framework Setup** (if Testing Strategy includes automated tests)

Based on Tech Stack ADR → Testing Strategy:

1. Install the test framework specified in the ADR
2. Install mock library if API mocking is needed
3. Create test directory structure per language conventions
4. Add test commands to package.json / pyproject.toml / Makefile / etc.
5. Verify setup works: run empty test suite

Skip this step only if ALL components in Testing Strategy are "Manual".

---

## Documentation

Populate all project documentation based on the prompt outputs.

---

### 1. docs/system.md

| Section | Source |
|---------|--------|
| What is [Name] | Requirements Summary → Project Overview |
| The problem | Requirements Summary → Core Problem |
| Who it's for | Requirements Summary → Target Users (Primary) |
| What it does | (leave empty - grows with completed features) |

---

### 2. project.md

| Field | Source |
|-------|--------|
| Project Name | Requirements Summary → Project Overview (extract name) |
| Active features | (leave empty) |
| TODO features | Requirements Summary → Functional Requirements |

---

### 3. docs/architecture.md

| Section | Source |
|---------|--------|
| System Overview (one-liner) | Requirements Summary → Project Overview |
| ASCII diagram | Architecture Document → System Diagram |
| Directory Structure | Tech Stack ADR → File/Folder Structure |
| Key Components | Architecture Document → Components table |
| Data Flow | Architecture Document → (generate from components) |
| External Integrations | Requirements Summary → Integration Points |
| System Boundaries | Requirements Summary → Security (public/auth/admin) |
| Key Decisions | Architecture ADR + Tech Stack ADR → summarize |
| Performance Considerations | Tech Stack ADR → Caching, indexing notes |
| Monitoring & Observability | Architecture ADR → Logging requirement |

---

### 4. docs/database.md

| Section | Source |
|---------|--------|
| Database type | Tech Stack ADR → Primary Database |
| ORM/Query library | Tech Stack ADR → Key Libraries |
| Location | Tech Stack ADR → Infrastructure |
| Schema tables | Architecture Document → Data Model |
| Query conventions | Generate for chosen database (PostgreSQL, SQLite, etc.) |
| Transaction pattern | Generate for chosen stack |

---

### 5. docs/decisions.md

Add the ADRs from the prompts:

```markdown
## ADR-001: Architecture Pattern

**Date:** [today]
**Status:** Accepted

### Context
[From Architecture ADR → Context section]

### Decision
[From Architecture ADR → Recommendation]

### Alternatives considered
[From Architecture ADR → Options Considered]

### Consequences
[From Architecture ADR → What we're trading away]

---

## ADR-002: Technology Stack

**Date:** [today]
**Status:** Accepted

### Context
[From Tech Stack ADR → Context section]

### Decision
[From Tech Stack ADR → Summary table]

### Alternatives considered
[From Tech Stack ADR → Options Considered per layer]

### Consequences
[From Tech Stack ADR → Risks]
```

---

### 6. docs/patterns.md

**Update code examples to match chosen language.**

| If Tech Stack includes | Add patterns |
|------------------------|--------------|
| JavaScript/TypeScript | async/await, Promise handling, Express middleware |
| Python | async def, FastAPI dependencies, SQLAlchemy sessions |
| Go | error handling, context propagation, defer patterns |
| C# | async Task, dependency injection, Entity Framework |

**Replace placeholder examples** (`api-[name]`, `data-[name]`) with stack-specific code.

---

### 7. docs/pitfalls.md

**Update code examples to match chosen language.**

| If Tech Stack includes | Add pitfalls |
|------------------------|--------------|
| JavaScript | unhandled Promise rejection, callback hell, `this` binding |
| Python | mutable default arguments, GIL issues, import cycles |
| Go | nil pointer dereference, goroutine leaks, channel deadlock |
| React | useEffect dependencies, stale closures, key prop issues |
| SQL | N+1 queries, missing indexes, SQL injection |

**Replace placeholder examples** with stack-specific code.

---

### 8. CLAUDE.md

| Section | Source |
|---------|--------|
| Project Name | Requirements Summary → Project Overview |
| Commands (install, dev, test, lint, build) | Tech Stack ADR → Key Decisions (scripts) |
| Testing | Tech Stack ADR → Development Workflow → Testing Strategy (copy the component table!) |
| Style rules | Generate for chosen stack (naming, formatting) |
| Pitfall triggers | Link to stack-specific pitfalls added above |
| Structure | Tech Stack ADR → File/Folder Structure |

**Important:** The Testing section must include the component-based testing table from the Tech Stack ADR. This is what `/implement` reads to know how to test each component.

---

### 9. .claude/settings.json

Generate hooks based on Tech Stack ADR:

| If Tech Stack includes | Generate hooks |
|------------------------|----------------|
| JavaScript/TypeScript | `npx prettier --write "$FILE"` on Edit/Write, `npm test` before commit |
| Python | `black "$FILE"` on Edit/Write, `pytest` before commit |
| Go | `gofmt -w "$FILE"` on Edit/Write, `go test ./...` before commit |
| Rust | `rustfmt "$FILE"` on Edit/Write, `cargo test` before commit |

**Template structure:**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "[formatter command from table above]"
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
            "command": "[test command from table above]"
          }
        ]
      }
    ]
  }
}
```

---

### Runtime templates

| File | Reason |
|------|--------|
| docs/inbox.md | Empty until bugs/ideas are logged |
| docs/features/_template.md | Template for new features |
| claude-progress.txt | Empty until first session |

### Agents

Agents in `.claude/agents/` are ready to use. They evolve automatically by reading pitfalls.md and patterns.md dynamically.

---

### Create first feature

```
/research [first feature from Requirements Summary → Functional Requirements]
```

---

## Output

Show the user:

```
## /setup complete

### Phase outputs
- Requirements Summary ✓
- Architecture ADR ✓
- Tech Stack ADR ✓
- Validation Report ✓
- Architecture Document ✓

### Verdict: [APPROVED/APPROVED WITH CONCERNS]

### Project files updated
- docs/system.md ✓ (what it is, problem, users)
- project.md ✓ (name, TODO features)
- docs/architecture.md ✓ (full system documentation)
- docs/database.md ✓ (schema, conventions)
- docs/decisions.md ✓ (ADR-001, ADR-002)
- docs/patterns.md ✓ (stack-specific examples)
- docs/pitfalls.md ✓ (stack-specific examples)
- CLAUDE.md ✓ (commands, style rules, structure)
- .claude/settings.json ✓ (hooks for formatting, testing)

### Runtime templates (empty until used)
- docs/inbox.md
- docs/features/_template.md
- claude-progress.txt

### Agents (ready to use, evolve automatically)
- .claude/agents/code-reviewer.md
- .claude/agents/doc-sync-checker.md
- .claude/agents/debugger.md
- .claude/agents/performance-optimizer.md

### Next step
Run: /research [first feature]
```

---

## Do NOT

- Skip any phase
- Suggest technologies before Discovery is complete
- Proceed with BLOCKED verdict
- Start coding before documentation is complete
- Rush through discovery (it's the most important phase)
