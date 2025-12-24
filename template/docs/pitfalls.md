# Pitfalls

> Mistakes we've made. Don't repeat them.

## How to use

**When writing code**: Check if your pattern matches any trigger below
**When stuck**: Read this file fully
**Before commit**: Verify no pitfall patterns in your code
**In code**: Add `// AI-CONTEXT: See docs/pitfalls.md#[category]-[name]`

## Anchor format

Use category-prefixed anchors from the start:
- `#async-unhandled-promise`
- `#security-sql-injection`
- `#database-missing-index`

This enables splitting later without changing code references.

## Categories

- **async** - Promises, timing, race conditions
- **security** - Auth, validation, injection
- **database** - Queries, transactions, migrations
- **api** - HTTP, responses, error handling
- **state** - Caching, consistency, side effects
- **error** - Exception handling, logging, fail-fast

---

## error-catch-all

**Trigger:** try/catch blocks, exception handling, error suppression

AI tends to generate catch-all handlers that swallow errors, making code "feel stable" while hiding problems.

❌ Wrong:
```csharp
try {
    await ProcessOrder(order);
} catch (Exception ex) {
    Logger.Log(ex);  // Swallows error, caller thinks success
}
```

✅ Right:
```csharp
try {
    await ProcessOrder(order);
} catch (Exception ex) {
    Logger.Log(ex);
    throw;  // Log AND re-throw
}
```

**Also:** Use guard clauses to validate inputs early. Return/throw immediately on invalid state.

---

---

## Templates (remove when adding real entries)

### async-[name]

**Trigger:** [Pattern that triggers this pitfall]

❌ Wrong:
```
[Code that causes problems]
```

✅ Right:
```
[Code that works correctly]
```

### security-[name]

...

---

## Adding new pitfalls

**Via /retro command (recommended):**
Use `/retro [description]` after a bug. This generates the correct format automatically.

**Manually:**
1. Add entry with category-prefixed anchor
2. Show wrong/right code, minimal explanation
3. Add trigger to CLAUDE.md
4. Add AI-CONTEXT comment in fixed code

## Entry format (from /retro)

Entries from `/retro` use plain language:

```markdown
---

## [category]-[short-name]

**Severity:** low | medium | high | critical
**Date:** YYYY-MM-DD

**What went wrong:**
[Description in plain language - what did the user see?]

**Why this could happen:**
[What check or test was missing?]

**Prevention:**
[One concrete, verifiable action]

**Trigger:** [When should AI think about this?]
```

**Severity levels:**
- **low** - Annoying but no damage
- **medium** - Feature doesn't work well
- **high** - Data lost or corrupted
- **critical** - Security or completely broken

---

## Scaling

**When to split:** >50 entries OR >15k tokens OR hard to scan

**How to split:**

1. Create `docs/pitfalls/` directory
2. Move entries to category files: `async.md`, `security.md`, etc.
3. Keep this file as index (see below)
4. **Code references don't change** - anchors stay the same

**This file becomes index:**

```markdown
# Pitfalls Index

Categories:
- [async](pitfalls/async.md) - Promises, timing, race conditions
- [security](pitfalls/security.md) - Auth, validation, injection
- [database](pitfalls/database.md) - Queries, transactions

Anchor format: `#[category]-[name]`
Files location: `docs/pitfalls/[category].md`
```

**After split, AI-CONTEXT still works:**
```
// AI-CONTEXT: See docs/pitfalls.md#security-sql-injection
→ AI reads index, finds security.md, locates #sql-injection
```
