# /implement [feature-name]

Execute tasks from a feature spec using test-first development.

> **Note:** `$ARGUMENTS` = the name you provide when running this command.
> Example: `/implement user-auth` → `$ARGUMENTS` becomes `user-auth`

## Instructions

Read these docs:
- docs/features/$ARGUMENTS.md (the spec - required)
- docs/pitfalls.md (always read before coding)
- docs/patterns.md (if spec references patterns)
- docs/architecture.md (if unsure where code belongs)
- claude-progress.txt (current state)

## Session management

**At start:**
1. Read claude-progress.txt (router) for quick resume
2. Read docs/features/$ARGUMENTS.md (source of truth) for full context
3. Update Status in feature spec to: In Progress

**During:**
- Update feature spec as you complete tasks
- Monitor context usage (stay under 60%)

**At end:**
- All tasks done? → Run `/complete` (cleanup, review, docs, wrap)
- Session ending early? → Run `/wrap` (progress only)

## Session limits

- **Context check**: If approaching 60%, commit and update progress
- **Checkpoint**: Periodically pause and validate (see Checkpoints section)

## Task execution loop

For each task in the spec:

### 1. Branch
```bash
git checkout main && git pull
git checkout -b feature/[name]-[task-id]
```

### 2. Testing (based on component type)

**Check CLAUDE.md → Testing section** for the project-specific testing strategy.

| Component type in CLAUDE.md | Approach |
|-----------------------------|----------|
| Unit tests | Write test first (TDD), then implement |
| Mocks + integration | Write implementation, then add mock tests |
| Manual + --dry-run | No automated tests, verify manually |
| (not listed) | Analyze the code - see below |

**If component not in testing table, analyze it:**

| If you're writing... | Then... |
|----------------------|---------|
| Pure function (no side effects, just transforms data) | Add unit test - these are cheap and valuable |
| API/external call | Add mock test |
| CLI glue code | Manual testing OK |

**Never skip testing silently.** If you think tests aren't needed for code that looks testable, say:
> "This is [type of code]. According to the testing strategy, [tests needed / manual OK]. Akkoord?"

**Use the approved test scenarios from the spec:**
1. Read the "Scenarios" field in the task (links to Test Scenarios section)
2. Look up those scenarios in the spec's "Test Scenarios" section
3. Translate each scenario to a test
4. Run test, confirm it fails (red) - for TDD approach
5. Commit: `[task-id]: Add test for [scenario name]`

**Do NOT invent new scenarios** - only implement what was approved in /plan.

### 3. Implement

```
"Now implement the code to make the test pass. Do NOT modify the test."
```

- Write minimal code to make test pass (green)
- Check against docs/pitfalls.md
- Add AI-CONTEXT comments where relevant:
  ```
  // AI-CONTEXT: See docs/patterns.md#[category]-[name]
  // AI-CONTEXT: See docs/pitfalls.md#[category]-[name]
  ```
- Run test, confirm it passes
- Commit: `[task-id]: Implement [what]`

### 4. Verify
- [ ] Test passes
- [ ] No pitfall patterns in code
- [ ] Lint passes
- [ ] No unrelated changes

### 5. Merge
```bash
git checkout main && git pull
git merge feature/[name]-[task-id]
git push
git branch -d feature/[name]-[task-id]
```

### 6. Update progress
- Check off completed task in spec
- Update claude-progress.txt

### 7. Next task
Repeat from step 1.

## Checkpoints (every 6 actions)

1. Still solving what spec describes?
2. Code matches pitfall patterns?
3. Main branch still working?
4. Adding things not in spec?
5. Context filling up? (if >60%, handover)
6. Progress recorded?

If concern: stop, discuss, update spec.

## Clean session ending

Before ending session or when context fills:

1. **Commit** working code (no half-implementations)
2. **Tests** must pass
3. **Run `/wrap`** automatically:
   - Catches missed bugs/ideas from conversation
   - Auto-detects fixed inbox items
   - Updates claude-progress.txt
4. **Handover** clear enough for fresh start

## Completion

When all tasks done:

Run `/complete [feature-name]`

This handles: cleanup, refactor, security check, code review, documentation sync, learnings capture, and moving spec to done/.

## Recovery

**Test won't pass (2 attempts):** Invoke the debugger agent:

```
Use a subagent with the instructions from .claude/agents/debugger.md to analyze why this test is failing: [test name and error]
```

The debugger will trace root cause and propose a fix. If still stuck after debugger analysis, the spec or test may be wrong.

**Merge conflict:** Resolve now. If complex, task boundaries were wrong.

**Scope creep:** Add to spec as future task. Don't expand current.

**Context filling:** Commit, update progress, start fresh session.

**Going in circles:** Commit what works, update progress, fresh session.

**Bug fixed:** If bug took >30min, data loss, or security → run `/retro [description]` to add lesson to pitfalls.md

## Do NOT

- Skip testing without checking CLAUDE.md testing strategy
- Skip testing silently (always confirm with user if unsure)
- Merge without tests when component type requires them
- Multiple tasks in one branch
- Modify files outside task's file list (except docs/inbox.md and claude-progress.txt)
- Continue past 60% context without checkpoint
- End session with uncommitted or broken code
