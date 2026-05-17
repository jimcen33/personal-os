# Changelog

All notable changes to this template will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

---

## [0.2.0] — 2026-05-17

### Added

- Full Chinese translations for all major documentation:
  - `README.zh-CN.md` (full, not stub)
  - `ARCHITECTURE.zh-CN.md`
  - `DECISIONS.zh-CN.md`
  - `docs/architecture/` — five-layer-model, three-operations, quality-gate, spotlight-rule, workstations (5 files)
  - `docs/customization/` — adding-a-workstation, writing-your-own-skill, tuning-quality-gate (3 files)
  - `docs/extending/multi-vault.zh-CN.md`
  - `docs/skills/` — README, one-file-per-skill (2 files)
  - `docs/why/` — karpathy-llm-wiki, jeff-su-cowork-os, synthesis (3 files)
- Worked-example branch (separate bundle): `worked-example/jordan-rivera`
  - Fictional creator-turned-product-builder persona
  - Filled CLAUDE.md, MEMORY.md, hot.md, voice-principles.md
  - 2 Knowledge pages (one with full Zone C decomposition)
  - 2 Notes/Inbox files mid-processing
  - 4 Notes/_Queue items at different lifecycle stages
  - 1 Software product plan
  - 1 LifeOS/Personal_Finances decision page
  - 1 Writing/Drafts brief seed turned partial draft
  - 1 Writing/Knowledge writing-craft methodology page
  - 1 Tutorial with full Verdict & Comparison block
  - Populated index.md, log.md, entity-dictionary.md, spotlight-log.md (12 entries with outcomes)
  - Populated Email HQ MEMORY.md with thread context

---

## [0.1.0] — 2026-05-17

### Added
- Initial public release (English-only).
- Root `CLAUDE.md` (constitution), `MEMORY.md` (durable facts stub), `AGENTS.md` (portable wiki schema).
- Five-layer folder structure: `Notes/`, `Knowledge/`, `Software/`, `LifeOS/`, `Writing/`.
- Workstations: `Writing HQ`, `Email HQ`, `Tutorials`.
- `00_Resources/` with stubs for hot cache, voice principles, index, log, promotion checklist, entity dictionary, autoresearch program, spotlight rule, spotlight log, cross-project bridge, prompt templates.
- Core skills: `ingest`, `queue-review`, `queue-triage`, `promote`, `lint`, `wiki`, `hot-cache`.
- Helper skills: `voice-extract`, `autoresearch`, `connections`, `new-info`, `sync-tasks`, `starter-session-audit`, `humanizer`, `tutorial-generator`.
- Top-level docs: `README.md`, `ARCHITECTURE.md`, `DECISIONS.md`, `CONTRIBUTING.md`.
- MIT `LICENSE`.
- `.github/` issue and PR templates.
- `docs/architecture/`, `docs/skills/`, `docs/customization/`, `docs/extending/`, `docs/why/` deep-dive pages.
