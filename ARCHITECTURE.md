# Architecture

A complete tour of how `personal-os` is structured and why each piece exists. If you want the *why these choices and not others*, read [DECISIONS.md](DECISIONS.md) after this.

---

## One-paragraph mental model

The wiki is a **5-layer pipeline** (Input → Knowledge → Skills → Action → Output) operated by **3 core operations** (`/ingest`, `/wiki <q>`, `/lint`), governed by **4 invariants** (Quality Gate, Spotlight Rule, Disposable Noise list, declared Wiki Scope), addressable through **slash-command skills**, and stored as plain Markdown so any agent can pick it up via `AGENTS.md`.

---

## The five layers

Every file lives in exactly one of these. They form a left-to-right pipeline.

| # | Layer | Folder | Lives here |
|---|---|---|---|
| 1 | Input | `Notes/` | Raw thoughts (`Inbox/`), web captures (`Clippings/`), AI conversations (`Conversation/`), 30-day evaluation queue (`_Queue/`) |
| 2 | Knowledge | `Knowledge/` | Durable understanding — methodologies, frameworks, book notes, original thinking |
| 3 | Skills | `Software/` | Tools, code, product/feature plans, dev notes |
| 4 | Action | `LifeOS/` | Decision-driving knowledge: Investing, Health, Insurance, Contacts, Personal Finances |
| 5 | Output | `Writing/` | Deliverables — drafts, published pieces, scripts, plus a `Writing HQ` workstation |

**Why the layers?** Each has a different *velocity* and *retention rule*. Notes are cheap and disposable. Knowledge is expensive and durable. Action layer demands accuracy (these notes drive real money / health / legal decisions). Output is where everything else converges. Mixing them dilutes both.

---

## The three operations

These are skills (`.claude/skills/<name>/SKILL.md`) — typed as slash-commands.

### `/ingest`
**Job:** Move raw material from `Notes/` into the right destination, applying the Quality Gate.

The structure of an ingested page is **three zones**:

- **Zone A — Verbatim.** Prompt templates, step sequences, frameworks, data tables. ≥40% of source substance. NEVER paraphrase.
- **Zone B — Metadata.** Cross-links, source URL, tags. Sits at the bottom.
- **Zone C — Decomposed SOP.** Only for methodology sources at reliability ≥ medium. Six mandatory subsections:
  1. 🩻 Core Question
  2. 🧽 Cut the Fluff
  3. 📉 Plain-Language Translation
  4. 🪜 Reverse-Engineered SOP
  5. 🛡️ Failure Modes
  6. 🧐 Critical Review

Zone C is the difference between a wiki of articles and a wiki of *operational knowledge*.

### `/wiki <question>`
**Job:** Search, synthesize, cite.

Returns an answer with `[[wikilinks]]` to every source. Bumps `last-retrieved` on cited pages (so `/lint` knows what's still alive). Surfaces missing cross-links the user can approve and patch.

### `/lint <layer>`
**Job:** Health-check one layer at a time. Reports only — no auto-fix.

Seven checks: orphans, stale (>90 days), contradictions (drops in-page `[!contradiction]` callouts), broken links, node decay, gaps (entities referenced ≥3× without a canonical page), Zone C drift (decomposed pages that drifted back to summary mode).

---

## The four governance rules

### 1. Quality Gate
Every ingest item must answer:
1. Will this still matter in 1 year?
2. Did it change my thinking, or just inform me?
3. Is it relevant to my declared scope or current Active Projects?
4. Will I realistically retrieve this later?

Score: 0–4. Below 3 → Queue (30-day expiry). At 3 or above → eligible for Permanent (subject to dispatch approval).

### 2. Spotlight Rule
Default behavior is **silence**. Only surface a high-value insight when a 3-factor weighted score crosses 7.0 and source confidence is ≥ medium.

Rubric (each 1–10):
- Leverage Type (× 0.4) — Naval's stack: code/media > capital > labor
- Specific Knowledge (× 0.3) — sharpens your defensible edge vs. generic
- First-Principles Surprise (× 0.3) — breaks an assumption vs. confirms what you knew

Anti-fatigue: max 3 spotlights per session. Every spotlight gets logged to `00_Resources/spotlight-log.md`. At 30 log entries, recalibrate the threshold based on outcome distribution.

This rule structurally prevents sycophancy — the surprise dimension forces the agent to score *against* the user's existing model.

### 3. Disposable Noise list
Materials the agent **refuses to file** unless the user says "save this anyway":
- AI model release announcements, benchmark scores
- Funding round news, valuations, acquisition gossip
- One-tweet "prompt hacks" without underlying principle
- "Industry trends in X" listicles
- Newsletter editions summarizable in one sentence
- Second-brain explainers when the user already runs the same pattern

### 4. Declared Wiki Scope
A 2–4 sentence charter in `CLAUDE.md` declaring what the wiki is *for*. Off-scope material defaults to queue or discard. This is the single most important text in the whole system — it gates everything else.

---

## Memory tier

Three files plus a hot cache:

- **`CLAUDE.md`** — Constitution. Rules, preferences, routing map, governance. Read every session.
- **`MEMORY.md`** — Durable facts. Identity, active projects, decisions, contacts, glossary.
- **`AGENTS.md`** — Portable wiki schema. Lets a non-Claude agent operate the wiki by reading this file alone.
- **`00_Resources/hot.md`** — Session cache. ~250 words on what's loaded right now. Refreshed by `/hot-cache` at end of heavy sessions.

---

## Workstations

Sub-systems with their own rules, voice, and resources. Each lives in a top-level folder with a `CLAUDE.md` that **layers on top of** the root `CLAUDE.md` — it doesn't replace it.

This template ships three:

- **Writing HQ** (`Writing/Writing HQ/`) — long-form drafting with a 9-step workflow (format confirm → brainstorm → outline → SEO lock → draft → voice pass → humanizer pass → image prompts → assemble).
- **Email HQ** (`Email HQ/`) — inbox triage, reply drafting, thread-aware response matching.
- **Tutorials** (`Tutorials/`) — turning techniques into non-engineer-friendly guides with a `TUTORIAL_TEMPLATE.md` and a `/tutorial <topic>` skill.

To add your own, see [docs/customization/adding-a-workstation.md](docs/customization/adding-a-workstation.md).

---

## Skills

All in `.claude/skills/<name>/SKILL.md`. Each skill has frontmatter (`name`, `description`, `last-updated`) and a body documenting trigger phrases, inputs, steps, outputs, failure modes.

Core skills shipped in this template:

| Skill | Purpose |
|---|---|
| `ingest` | Quality-gated capture with three-zone format and Zone C decomposition |
| `queue-review` | Weekly: items expiring in next 7 days |
| `queue-triage` | Bulk: full-queue scan with PROMOTE/EXTEND/SPLIT/EXPIRE recommendations |
| `promote` | Independent validator. **Must run in a fresh session.** |
| `lint` | Seven-check audit of one layer |
| `wiki` | Search + synthesize + cite + surface missing links |
| `hot-cache` | Refresh `00_Resources/hot.md` at session end |
| `autoresearch` | 3-round web research with gap-filling |
| `voice-extract` | Pull writing patterns from your samples into `voice-principles.md` |
| `connections` | Find cross-idea connections, output writing brief seeds |
| `new-info` | Verified current-info lookup (refuses to hallucinate UI paths, dates) |
| `sync-tasks` | Detect drift between memory files and scheduled tasks |
| `starter-session-audit` | End-of-session scan for uncaptured corrections / preferences |
| `humanizer` | Remove AI-writing tells from drafts |

---

## Data flow examples

### "I read an interesting article."
1. Paste into `Notes/Inbox/`.
2. `/ingest` → agent runs Quality Gate.
3. Score < 3 → routed to `Notes/_Queue/` with 30-day expiry. Done.
4. Score ≥ 3, methodology source → routed to `Knowledge/` with Zone C decomposition. Added to `00_Resources/index.md`, logged in `00_Resources/log.md`.

### "What do I know about retention strategy?"
1. `/wiki retention strategy`
2. Agent searches `Knowledge/`, then `Software/`, then `Writing/Knowledge/`.
3. Synthesizes answer with `[[wikilinks]]` to source pages.
4. Bumps `last-retrieved` on each cited page.
5. Notices two pages that should link but don't → asks user to confirm → patches the link.

### "My queue is getting unwieldy."
1. `/queue-triage`
2. Agent reads every file in `Notes/_Queue/`, parses frontmatter, pulls TL;DR.
3. Recommends action per item (PROMOTE / EXTEND / SPLIT / EXPIRE) with one-line reasoning.
4. Groups connected items by shared entities for batch promotion.
5. Generates dispatch lists. User applies them.

---

## What's deliberately NOT in here

- **No database.** Plain Markdown only. The "schema" is frontmatter + folder convention.
- **No global search index.** Files are small enough that grep + agent-driven search wins.
- **No auto-fixing.** `/lint` reports; the user fixes. Agency over content stays with the human.
- **No required cloud.** Run it on local disk, iCloud, Dropbox, or anywhere — the system is path-agnostic.
- **No auto-promote.** Queue → Permanent always goes through `/promote` in a fresh session, by design.

---

## Going further

- [DECISIONS.md](DECISIONS.md) — Why these specific choices, what alternatives were rejected
- [docs/architecture/five-layer-model.md](docs/architecture/five-layer-model.md) — Deep dive on the 5 layers
- [docs/architecture/three-operations.md](docs/architecture/three-operations.md) — Deep dive on ingest/query/lint
- [docs/architecture/quality-gate.md](docs/architecture/quality-gate.md) — Why a 4-question gate, not 5 or 3
- [docs/architecture/spotlight-rule.md](docs/architecture/spotlight-rule.md) — The anti-sycophancy mechanism
- [docs/architecture/workstations.md](docs/architecture/workstations.md) — When to add one, when to merge
- [docs/extending/multi-vault.md](docs/extending/multi-vault.md) — Growing into a constellation of vaults
