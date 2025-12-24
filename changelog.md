# Changelog

All notable changes to the AI Development Playbook.

Format: [Semantic Versioning](https://semver.org/)
- **Major** (1.0 → 2.0): Breaking changes, major restructuring
- **Minor** (1.0 → 1.1): New features, new commands
- **Patch** (1.0.0 → 1.0.1): Bugfixes, small improvements

---

## [2.0.0] - 2024-12-24

### Breaking change: template/ folder structure

All project files now live in a `template/` folder. This prevents confusion between playbook-meta (readme, version, changelog) and project files (CLAUDE.md, docs/, etc.).

**What changes:**
- `version` → `template/.playbook-version` (renamed to avoid confusion)
- All project files moved to `template/`
- `/playbook-update` now reads `.playbook-version` instead of `version`

**For existing projects:**
1. Rename `version` to `.playbook-version` in your project root
2. Done! Nothing else needed.

**For new projects:**
- Copy only the `template/` folder to your project

---

## [1.1.0] - 2024-12-24

### New feature: system.md

Central documentation for "what are we building?" - a holistic project description that grows with each completed feature.

**Contains:**
- `docs/system.md` template with 4 sections: What is, The problem, Who it's for, What it does
- `/setup` fills first 3 sections automatically from Discovery output
- `/complete` adds features to "What it does" section

**What to do:**
1. Copy `docs/system.md` to your project
2. Fill in after Discovery phase (or let /setup do it)

---

## [1.0.0] - 2024-12-24

### Initial release

Initial version of the AI Development Playbook.

**Contains:**
- Setup prompts (discovery, architecture, tech-stack, validator)
- Workflow commands (/setup, /plan, /implement, /complete, /wrap)
- Documentation templates (architecture, patterns, pitfalls)
- Specialized agents (debugger, code-reviewer, doc-sync-checker)

**Key features:**
- Automatic testing strategy determination
- Rich idea capture with mini feature specs
- Session management with progress tracking
- "No silent skipping" rule - Claude asks before deviating from the process

---

## Template for future updates

<!--
## [X.Y.Z] - YYYY-MM-DD

### New feature: [name]
[Description of what it does and why]

**What to do:**
```
[Exact steps or code to copy]
```

### Bugfix: [description]
[What was the problem]

**What to do:**
```
[Exact steps to fix]
```

### Breaking change: [description]
[What changes and why]

**What to do:**
```
[Migration steps]
```
-->
