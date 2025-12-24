# /playbook-update

Check for updates to the AI Development Playbook and show what's new.

## Instructions

### Step 1: Read local version

Read the `version` file in the project root.
- If not found: assume version 0.0.0 (never updated)
- Store as `local_version`

### Step 2: Fetch latest from GitHub

Fetch these files from GitHub:
- https://raw.githubusercontent.com/mvletter/ai-development-playbook/master/version
- https://raw.githubusercontent.com/mvletter/ai-development-playbook/master/changelog.md

Store as `remote_version` and `changelog`.

### Step 3: Compare versions

If `remote_version` == `local_version`:
```
✓ Je playbook is up-to-date (versie X.Y.Z)
```
Done.

If `remote_version` > `local_version`:
```
📢 Playbook update beschikbaar!

Jouw versie: [local_version]
Nieuwste versie: [remote_version]
```

### Step 4: Show relevant changes

Parse the changelog.md and show only entries NEWER than `local_version`.

Present in plain language:
```
## Wat is er nieuw sinds jouw versie?

[Show changelog entries between local_version and remote_version]
```

### Step 5: Guide the user

After showing changes, say:

```
De changelog hierboven beschrijft wat er nieuw is en hoe je het kunt toepassen.

Lees de "Wat te doen" secties en pas toe wat relevant is voor jouw project.

Als je klaar bent, update ik je version file zodat je deze melding
niet opnieuw krijgt.

Klaar met reviewen? (j/n)
```

If yes:
1. Update the local `version` file to `remote_version`
2. Confirm: "✓ version bijgewerkt naar [remote_version]"

If no:
- "Geen probleem. Run `/playbook-update` wanneer je klaar bent om te reviewen."

## Important

- NEVER automatically change any file except version
- NEVER try to merge or diff project files
- The changelog contains human-readable instructions
- User decides what to apply to their project

## Error handling

If GitHub fetch fails:
```
⚠️ Kon de playbook repo niet bereiken. Check je internetverbinding.

Je kunt ook handmatig checken:
https://github.com/mvletter/ai-development-playbook/blob/master/changelog.md
```
