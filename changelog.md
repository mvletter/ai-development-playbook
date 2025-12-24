# Changelog

Alle belangrijke wijzigingen aan het AI Development Playbook.

Formaat: [Semantic Versioning](https://semver.org/)
- **Major** (1.0 → 2.0): Breaking changes, grote herstructurering
- **Minor** (1.0 → 1.1): Nieuwe features, nieuwe commands
- **Patch** (1.0.0 → 1.0.1): Bugfixes, kleine verbeteringen

---

## [1.1.0] - 2024-12-24

### Nieuwe feature: system.md

Centrale documentatie voor "wat bouwen we eigenlijk?" - een holistische beschrijving van het project die groeit met elke afgeronde feature.

**Bevat:**
- `docs/system.md` template met 4 secties: What is, The problem, Who it's for, What it does
- `/setup` vult eerste 3 secties automatisch in vanuit Discovery output
- `/complete` voegt features toe aan "What it does" sectie

**Wat te doen:**
1. Kopieer `docs/system.md` naar je project
2. Vul in na Discovery fase (of laat /setup het doen)

---

## [1.0.0] - 2024-12-24

### Eerste release

Initiele versie van het AI Development Playbook.

**Bevat:**
- Setup prompts (discovery, architecture, tech-stack, validator)
- Workflow commands (/setup, /plan, /implement, /complete, /wrap)
- Documentation templates (architecture, patterns, pitfalls)
- Specialized agents (debugger, code-reviewer, doc-sync-checker)

**Key features:**
- Automatic testing strategy determination
- Rich idea capture with mini feature specs
- Session management with progress tracking
- "No silent skipping" rule - Claude vraagt voordat ie afwijkt van het proces

---

## Template voor toekomstige updates

<!--
## [X.Y.Z] - YYYY-MM-DD

### Nieuwe feature: [naam]
[Beschrijving van wat het doet en waarom]

**Wat te doen:**
```
[Exacte stappen of code om te kopiëren]
```

### Bugfix: [beschrijving]
[Wat was het probleem]

**Wat te doen:**
```
[Exacte stappen om te fixen]
```

### Breaking change: [beschrijving]
[Wat verandert er en waarom]

**Wat te doen:**
```
[Migratie stappen]
```
-->
