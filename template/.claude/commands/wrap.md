# /wrap

End-of-session analysis. Catches items that weren't logged during the session.

## When to use

- End of work session
- Before running `/clear`
- When context is getting full (>50%)
- Automatically called by `/complete`

## Instructions

Read these docs:
- claude-progress.txt (router - check active context type)
- If Feature: docs/features/[name].md (source of truth)
- docs/inbox.md (for bug/idea logging)

## Protocol

### Phase 1: Auto-detect resolved inbox items

**CRITICAL:** Read docs/inbox.md first, then scan conversation to find matches.

For each open bug in inbox.md:
1. Check if this session touched related code/functionality
2. Check if error messages from this bug no longer appear
3. Check if user confirmed something "works now" / "fixed" / "solved"

**Match criteria:**
- Same error message mentioned and resolved
- Same functionality discussed and now working
- User explicitly said it's fixed
- Code was changed that directly addresses the bug

**If match found:** Mark as resolved (will be processed in Phase 4)

### Phase 2: Find unlogged items

Scan the ENTIRE conversation for:

**Bugs mentioned but not logged:**
- Error messages discussed
- "This doesn't work" / "broken" / "failing"
- Workarounds applied
- Edge cases discovered

**Ideas mentioned but not logged:**
- "We could..." / "Maybe we should..." / "What if..."
- Future improvements discussed
- Nice-to-haves mentioned
- Alternative approaches considered but not taken

**For each idea, assess detail level:**
- **Simple idea** (just mentioned, no details) → one-liner for inbox
- **Discussed idea** (technical approach, files, API design discussed) → create mini feature spec

### Phase 3: Present findings

Show the user:

```
## Session wrap-up

### Found in this session:

🐛 Bugs (not yet in inbox):
- [description] - [suggested severity]

💡 Ideas (not yet in inbox):
- [description] - [suggested priority] - **simple** (inbox only)
- [description] - [suggested priority] - **discussed** (will create spec)

✅ Resolved:
- [bug/item that was fixed but not marked]

### Already tracked:
- [X bugs in inbox]
- [Y ideas in inbox]
```

**For "discussed" ideas, show what will be captured:**
```
📝 [idea name] spec will include:
- Problem: [what we discussed]
- Approach: [technical details mentioned]
- Files: [files/components mentioned]
- Open questions: [if any]
```

### Phase 4: Confirm and log

Ask: "Add these to inbox? (y/n or edit)"

If yes:
1. Add bugs to docs/inbox.md with next available #
2. For **simple ideas**: Add one-liner to docs/inbox.md
3. For **discussed ideas**:
   - Create `docs/features/[idea-name].md` using template with Status: Idea
   - Fill in: Problem, Approach (what was discussed), Files mentioned
   - Leave other sections empty (to be filled by /research later)
   - Add to inbox.md with link: `[idea] → [spec](features/[name].md)`
4. Mark resolved items as processed
5. Update claude-progress.txt with session summary

### Phase 5: Update tracking

**If Feature:**
1. Update feature spec with session notes
2. Add entry to claude-progress.txt session history:
   ```
   ### [YYYY-MM-DD]
   - Type: Feature
   - Context: [feature name]
   - Completed: [tasks done]
   - Handover: [what's next]
   ```
3. Update "Quick resume" section

**If Maintenance/Bug (inline):**
1. Update "Current work" checkboxes in claude-progress.txt
2. Add entry to session history
3. Update "Quick resume" section

## Output

Show confirmation:

```
✅ Session wrapped

Added to inbox:
- 🐛 2 bugs (#4, #5)
- 💡 1 idea (#3) - simple
- 💡 1 idea (#4) → created spec: docs/features/[name].md

Marked as processed:
- Bug #2 (fixed)

Progress updated. Ready for /clear or continue.
```

## Do NOT

- Skip conversation analysis
- Add items without user confirmation
- Forget to update inbox.md
- Leave claude-progress.txt outdated
