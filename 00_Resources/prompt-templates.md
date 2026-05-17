---
last-updated: 2026-05-17
---

# Prompt Templates

Copy-paste prompts for the most common operations. Use these when the skill-driven flow isn't loaded (e.g., you're in a different LLM client without skill-loading support).

---

## Ingest

```
You are operating my personal wiki, which uses a 5-layer architecture (Notes/Knowledge/Software/LifeOS/Writing) with a 4-question Quality Gate. Architecture is documented in AGENTS.md.

Here is the file to ingest:

<paste content>

Steps:
1. Refuse if this is Disposable Noise (model releases, funding gossip, one-tweet prompt hacks, listicles, second-brain explainers).
2. Run the 4-question Quality Gate. Report gate-score (0-4) and gate-failures.
3. Propose a dispatch table: file → target layer + reasoning + Decompose Y/N.
4. WAIT for my approval before writing anything.
5. On approval: write the destination file in three zones (Verbatim / Metadata / Decomposed SOP if applicable).
6. Append a log entry.

Do not auto-promote. Do not skip the wait.
```

---

## Query

```
You are answering a question from my personal wiki. Architecture in AGENTS.md.

Question: <your question>

Steps:
1. Identify the relevant layer(s).
2. Search file names, frontmatter tags, then full text.
3. Synthesize an answer with [[wikilinks]] to source pages.
4. For each cited page, note that last-retrieved should be bumped to today.
5. If you spot two pages that should link but don't, surface the connection. I will confirm before you patch.
6. Flag reliability: low or medium citations.
```

---

## Lint

```
You are linting a single layer of my personal wiki. Architecture in AGENTS.md.

Layer: <Knowledge | Software | LifeOS | Writing | Notes>

Run these seven checks and report findings (do not auto-fix):

1. Orphans — pages with zero inbound [[wikilinks]]
2. Stale — last-updated > 90 days ago
3. Contradictions — drop in-page [!contradiction] callouts when found
4. Broken links — wikilinks pointing to nonexistent pages
5. Node decay — last-retrieved > 90 days
6. Gaps — entities referenced ≥3× without a canonical page
7. Zone C drift — type: decomposed pages where the SOP section drifted back to summary mode

Report format: section per check, with affected page paths and one-line description.
```

---

## Promote (run in a fresh session)

```
You are validating a Queue item for promotion to a Permanent layer in my personal wiki. Architecture in AGENTS.md. Promotion checklist in 00_Resources/promotion-checklist.md.

Queue item path: <path>
Proposed destination layer: <Knowledge | Software | LifeOS | Writing>
Proposed type: <methodology | concept | decision | reference | decomposed>

You have no prior context on this item. Score it cold.

Run the promotion checklist:
1. Cold re-score the 4-question Quality Gate. Pass threshold: 4/4 for Permanent.
2. Entity dictionary check (00_Resources/entity-dictionary.md) — is there a duplicate?
3. Type validation — does the content support the proposed type?
4. Layer fit — is the proposed layer correct?
5. Cross-link discovery — surface suggested cross-links.

Output: PROMOTE | MERGE | EXTEND | EXPIRE | DECOMPOSE_FIRST | SPLIT with reasoning.
```

---

## Queue review (weekly)

```
You are running a weekly queue review on my personal wiki.

List every file in Notes/_Queue/ where `expires` is within the next 7 days. For each, parse frontmatter, pull a TL;DR from the body, and present:

- Filename
- gate-score, gate-failures
- expires (date)
- TL;DR (one line)
- Recommendation: PROMOTE | EXTEND (+30d) | EXPIRE

I will respond per-item. Do not act until I confirm each.
```

---

## Hot cache refresh (end of session)

```
You are refreshing my hot cache for the next session.

Look back at this session's work. Rewrite 00_Resources/hot.md (~250 words) with:

## Refreshed: <today>

### What I'm working on right now
- <3–5 bullets from this session>

### Recent decisions (last 2 weeks)
- <bullets>

### Up next
- <bullets>

### Stale / about to fall off
- <bullets from MEMORY.md Active Projects that haven't been touched in 14+ days>
```

---

## Voice extract

```
You are extracting my writing voice from samples. Update 00_Resources/voice-principles.md.

Here are 5–10 of my recent sent emails / posts / DMs:

<paste samples>

Detect patterns:
- Sentence shape (average length, structure)
- Word choices (preferred + avoided)
- Punctuation style
- Structural preferences (openings, paragraph length, bullet vs prose)
- Banned phrases (corporate-speak, clichés you'd never say)

Update voice-principles.md preserving the existing structure. Surface anything ambiguous so I can correct.
```
