# Decisions

What I borrowed, what I changed, what I invented, and the alternatives I rejected. If you're going to fork this, read this file first — it'll tell you which parts are load-bearing and which you can replace.

---

## Provenance

This system is a fusion of two prior ideas, plus the connective tissue I had to invent to make them work as one artifact.

### From Andrej Karpathy — the "LLM Wiki" concept
- Typed, structured pages (vs. free-form notes)
- Agent-readable schema (`AGENTS.md`)
- Quality bar over quantity (refuse low-value material)
- Wiki as a *substrate for an agent*, not just a search target

### From Jeff Su — the "Personal OS / Cowork OS" pattern
- Workstations (sub-systems with their own CLAUDE.md)
- Hot cache for session continuity
- Plain-language interfaces (the user types human, not config)
- Five-layer pipeline (Input → Knowledge → Skills → Action → Output)

### What I invented (or borrowed-and-reshaped) to make them fit
- **The 4-question Quality Gate** — discrete, scorable, blocks promotion to Permanent
- **Three-zone ingest format** (verbatim / metadata / decomposed SOP)
- **Zone C decomposition** — the methodology-to-SOP transform with 6 mandatory subsections
- **The Spotlight Rule** — anti-sycophancy gating with a calibration log
- **The Disposable Noise refusal list** — explicit list of what NOT to file
- **Multi-vault routing with sandbox constraints** — the constellation pattern (deferred to `docs/extending/`)
- **Quality-gate-first ingest** instead of capture-first / sort-later

---

## Decisions (chronological-ish)

### D-001: Plain Markdown, no database
**Status:** Accepted.
**Context:** Most "second brain" systems lock you into a tool (Notion, Roam, Tana, Obsidian-with-plugins). I wanted portability: any agent, any editor, any sync.
**Decision:** Files only. Folder convention + frontmatter is the schema.
**Trade-off:** Lose: instant property queries, bidirectional graph view, plugin ecosystems. Gain: portability, version-control friendliness, agent-readability.
**Rejected alternatives:**
- Notion API + LLM (too coupled to Notion's evolving API surface)
- SQLite-backed wiki (over-engineered for personal scale)
- Obsidian vault with heavy plugin stack (Obsidian-specific lock-in)

### D-002: Five layers, not three or seven
**Status:** Accepted.
**Context:** Karpathy's model is 3 layers (Input → Knowledge → Output). Jeff Su's is 5 (with a Skills and an Action layer separately).
**Decision:** Five. Skills (`Software/`) and Action (`LifeOS/`) have different retention rules and audiences from Knowledge — collapsing them dilutes the type signal.
**Trade-off:** More folders to remember. But the gate question "where does this go?" becomes more answerable, not less, because the boundaries are sharper.

### D-003: Quality Gate runs *before* storage, not after
**Status:** Accepted.
**Context:** Default capture-first systems (Roam, Logseq, Notion) optimize for low-friction capture. The cost is a graveyard of mediocre pages.
**Decision:** Items fail the gate go to `_Queue/` with a 30-day timer. They don't enter Permanent until they pass a `/promote` validation in a fresh session.
**Why this matters:** It's the single highest-leverage rule in the whole system. Most "second brain" pain is from low-quality content drowning the good stuff.
**Rejected alternatives:**
- Capture-everything-then-prune (relies on motivation you won't have)
- Gate at retrieval (too late — by then the noise has already cost search time)

### D-004: Promote in a *fresh* session
**Status:** Accepted.
**Context:** An LLM that just ingested an item is biased to over-value it.
**Decision:** Promotion requires a separate session where the agent has no context except the queue item and the rubric.
**Trade-off:** Adds friction. But the friction is the point — it's a sanity check on your own enthusiasm.

### D-005: Zone C decomposition (the 6-step SOP transform)
**Status:** Accepted.
**Context:** Most knowledge-base pages drift toward "this article discusses X" summary mode, which is useless when you actually need to *do* the thing.
**Decision:** Methodology sources get a mandatory 6-step decomposition: Core Question → Cut the Fluff → Plain-Language Translation → Reverse-Engineered SOP → Failure Modes → Critical Review.
**Refusal heuristic:** If the agent finds itself writing "this article discusses X," stop and ask "what does the reader DO Monday morning because of this?"
**Trade-off:** Decomposition takes 5-10× the tokens of a summary. Worth it for the pages you'll actually use.

### D-006: Spotlight Rule with 3-factor weighted score
**Status:** Accepted.
**Context:** Default LLM behavior is to surface lots of "you might find this interesting" — flattering, useless, and trains you to ignore the assistant.
**Decision:** Spotlight only fires when (Leverage × 0.4) + (Specific Knowledge × 0.3) + (Surprise × 0.3) ≥ 7.0 AND confidence ≥ medium. Max 3 per session.
**Anti-sycophancy mechanism:** The "Surprise" dimension forces the agent to score *against* the user's existing model. A spotlight that just confirms what you knew can't pass.
**Calibration loop:** Every spotlight is logged with a blank `outcome:` field. After 30 entries, lint the log — if <30% had positive outcomes, raise the threshold; if >70%, lower it.
**Trade-off:** Quieter assistant. Some users will mistake this for "Claude got worse." It's the correct trade.

### D-007: Default routing is Queue, not Permanent
**Status:** Accepted.
**Context:** See D-003. Bias should be toward *less* in Permanent, not more.
**Decision:** Even gate-passing items can be queued for further evaluation if the agent isn't sure of the layer assignment. Promotion is always explicit.

### D-008: No auto-fix on lint
**Status:** Accepted.
**Context:** Auto-fix is tempting (the agent could rename pages, patch broken links, dedupe). It's a trap — the agent's mental model of which page is canonical is often wrong.
**Decision:** Lint reports. Human fixes. Or human says "go ahead with the proposed plan" after reading the report.

### D-009: Single vault by default, constellation as opt-in
**Status:** Accepted.
**Context:** A real founder workflow typically grows into Personal + Company + Private vaults with cross-vault routing, sandbox constraints, and sensitivity-flagged auto-routing.
**Decision:** The template ships single-vault. The multi-vault pattern is documented in `docs/extending/multi-vault.md` for users who outgrow single.
**Trade-off:** Slightly less impressive demo. But onboarding is dramatically simpler, and 80% of users never need multi-vault.

### D-010: Workstations as folders with their own CLAUDE.md
**Status:** Accepted.
**Context:** Some tasks (drafting an email, writing a long-form piece, prepping a tutorial) need *layered* rules: the root constitution plus domain-specific voice and workflow.
**Decision:** Workstations are folders. Their CLAUDE.md layers on top of the root. Root provides the universal rules; workstation provides the domain rules.
**Pattern:** Each workstation has `CLAUDE.md`, `MEMORY.md`, and `<Name> Resources/`.
**Trade-off:** More files. But it scales — adding a workstation doesn't require editing the root constitution.

### D-011: Ship all skills (~17), not a minimal core
**Status:** Accepted.
**Context:** Users overwhelmed by 17 skills will use 5. Users given only 5 will never discover the other 12.
**Decision:** Ship everything; explain in README which 5–7 are core and which are situational. Users delete what they don't need.

### D-012: Spotlight Rule is core, not optional
**Status:** Accepted (debated).
**Context:** It's the most opinionated piece in the system. Some users will hate it.
**Decision:** Core default. The README is explicit that this is opinionated, and users can comment out the section in `CLAUDE.md` to disable.
**Reasoning:** The system's main differentiator is *governance* — opinionated rules that keep the wiki healthy. Stripping the most opinionated piece neuters the differentiator.

### D-013: MIT license
**Status:** Accepted.
**Context:** Choosing between MIT, Apache 2.0, CC BY-SA, CC0.
**Decision:** MIT.
**Reasoning:** Maximum reuse, including commercial. The patent grant in Apache 2.0 isn't relevant here (no inventions). CC BY-SA limits derivative-work licensing in ways that hurt adoption. CC0 forfeits attribution.

### D-014: Active maintenance, PRs welcome
**Status:** Accepted.
**Context:** Choosing maintenance posture: active vs. reference-only vs. snapshot.
**Decision:** Active, with explicit PR template and contributor guide.
**Trade-off:** Higher ongoing cost. But the system gets better when users push back with edge cases I haven't hit.

---

## Things I'm still unsure about

- **Should `voice-extract` be in core or optional-modules?** It's the most personal layer. Currently core because the README is explicit about running it on day one.
- **`/connections` weekly cadence vs. on-demand.** Currently on-demand. Some users will benefit from a scheduled `/connections` run; I haven't decided whether to ship a cron-style scheduling helper.
- **Multi-language voice principles.** The system assumes one voice per user, but bilingual creators may want two. Currently unsolved.

If you have opinions on any of these, open an issue.

---

## Rejected ideas worth naming

- **A graph view.** Tempting, but the graph is implicit in `[[wikilinks]]` + frontmatter. Building a visualization is a separate tool, not a feature of the wiki.
- **AI auto-tagging.** Tested; agent drifts to over-tag (every page gets "productivity" / "ai" / "knowledge"). Manual + lint catches the gaps reliably.
- **Forced bidirectional links.** Forces wiki-style symmetric linking everywhere. Costs more than it's worth at personal scale.
- **A REPL / CLI wrapper.** Adds a dependency. The whole point is "files + slash commands in your existing agent client."
