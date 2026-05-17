# The Five-Layer Model

> A deep dive on the five layers. For the high-level overview, see [ARCHITECTURE.md](../../ARCHITECTURE.md).

## Why five, not three or seven

Three-layer models (Input -> Knowledge -> Output) are clean but lose the distinction between *understanding* and *doing*. A page on "how compound interest works" (Knowledge) and a page on "my Roth IRA contribution schedule for 2026" (Action) have radically different retention rules, audiences, and update cadences. Collapsing them dilutes both.

Seven-layer models (adding sub-layers for things like "ephemeral", "in-progress", "archived") trade simplicity for granularity. The question "where does this go?" should have a fast answer.

Five hits the sweet spot for solo founders and creators.

## Layer-by-layer

### 1. Input - `Notes/`

The single funnel for all new material. Four sub-folders:
- `Inbox/` - raw thoughts, clipboard dumps, voice-memo transcripts
- `Clippings/` - web articles, screenshots, PDFs
- `Conversation/` - AI conversation exports worth keeping
- `_Queue/` - items that passed initial scrutiny but await promotion (30-day timer)

**Retention rule:** Inbox/Clippings/Conversation are processed by `/ingest` and emptied. `_Queue/` items expire automatically after 30 days unless promoted.

**Audience:** No one but you and the agent. This layer is private working space.

### 2. Knowledge - `Knowledge/`

Durable understanding. Methodologies, frameworks, book notes, original thinking. The slowest-changing layer.

**Retention rule:** Pages don't expire. Stale pages (`last-updated` >90 days for evergreen content is fine; check `last-retrieved` instead for relevance signal).

**Audience:** Future you, possibly other agents you've authorized.

### 3. Skills - `Software/`

Tools, code, product/feature plans, dev notes. Anything technical or product-related.

**Retention rule:** Tools change fast - flag pages stale at 90 days. UI-specific tutorials are subject to the most aggressive freshness review.

**Audience:** Future you when you need a specific how-to. Possibly collaborators if you cross-link to a shared repo.

### 4. Action - `LifeOS/`

Knowledge that drives real-world decisions: money, health, contacts, insurance, legal. The highest-stakes layer.

**Retention rule:** These pages get cross-verified before promotion. Stale rules are dangerous - flag at 90 days, demand re-verification.

**Audience:** Primarily you. The agent may use these to inform decisions but should always cite + flag reliability.

### 5. Output - `Writing/`

Drafts (`Drafts/`), published work (`Published/`), writing-craft methodology (`Knowledge/` - the *meta* writing knowledge), and a `Writing HQ` workstation.

**Retention rule:** Drafts move to `Published/` when shipped. Published doesn't expire.

**Audience:** The world (if shipped) or your future self (if kept private).

## What goes where - the dispatch logic

When ingest is unsure, it asks. The rough heuristic:

| Question | If yes... |
|---|---|
| Drives a money / health / legal / contact decision? | `LifeOS/` |
| About a tool, language, framework, codebase? | `Software/` |
| About a methodology, concept, or pattern? | `Knowledge/` |
| A deliverable or piece of writing? | `Writing/` |
| Writing-craft methodology specifically? | `Writing/Knowledge/` |
| None of the above, but worth keeping? | `Notes/_Queue/` |
| Disposable noise? | Refuse |

## Sub-layer extension

If a layer grows too large to navigate, sub-folder it by topic. `LifeOS/Investing/`, `LifeOS/Health/`, etc. are pre-created sub-folders. `Knowledge/` and `Software/` start flat - sub-folder when you hit ~30 pages in one area.

Don't sub-folder pre-emptively. Empty sub-folders signal "you should be filing here" and nudge ingest toward over-filing.

## Anti-patterns

- **Filing into Notes/ as a parking lot.** `Notes/` is a funnel, not a destination. Items there should be moving toward a destination layer or expiring.
- **Knowledge/ as a clippings folder.** If a page reads like an article rather than original synthesis, it doesn't belong in Knowledge. Decompose it into Zone C SOP form, or downgrade to reference type, or queue it.
- **Software/ holding decisions about software you'll never use again.** Lint catches these via the Node Decay check.
- **LifeOS/ holding research, not decisions.** The decisive question: "if this is wrong, do I lose money / harm my health / break the law?" If yes, LifeOS. If no, probably Knowledge.
