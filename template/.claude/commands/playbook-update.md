# /playbook-update

Check for updates to the AI Development Playbook and show what's new.

## Instructions

### Step 1: Read local version

Read the `.playbook-version` file in the project root.
- If not found: assume version 0.0.0 (playbook copied before versioning existed)
- Store as `local_version`

### Step 2: Fetch latest from GitHub

Fetch these files from GitHub:
- https://raw.githubusercontent.com/mvletter/ai-development-playbook/master/version
- https://raw.githubusercontent.com/mvletter/ai-development-playbook/master/changelog.md

Store as `remote_version` and `changelog`.

### Step 3: Compare versions

If `remote_version` == `local_version`:
```
✓ Your playbook is up-to-date (version X.Y.Z)
```
Done.

If `remote_version` > `local_version`:
```
📢 Playbook update available!

Your version: [local_version]
Latest version: [remote_version]
```

### Step 4: Show relevant changes

Parse the changelog.md and show only entries NEWER than `local_version`.

Present in plain language:
```
## What's new since your version?

[Show changelog entries between local_version and remote_version]
```

### Step 5: Guide the user

After showing changes, say:

```
The changelog above describes what's new and how to apply it.

Read the "What to do" sections and apply what's relevant to your project.

When you're done, I'll update your .playbook-version file so you won't see
this message again.

Done reviewing? (y/n)
```

If yes:
1. Update the local `.playbook-version` file to `remote_version`
2. Confirm: "✓ .playbook-version updated to [remote_version]"

If no:
- "No problem. Run `/playbook-update` when you're ready to review."

## Important

- NEVER automatically change any file except .playbook-version
- NEVER try to merge or diff project files
- The changelog contains human-readable instructions
- User decides what to apply to their project

## Error handling

If GitHub fetch fails:
```
⚠️ Could not reach the playbook repo. Check your internet connection.

You can also check manually:
https://github.com/mvletter/ai-development-playbook/blob/master/changelog.md
```
