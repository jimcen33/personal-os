# The Three Operations

The wiki has exactly three core operations. Everything else builds on these.

## 1. Ingest

**Purpose:** Move raw material into the right destination layer.

**Triggered by:** `/ingest`, "process my notes", "file this", new content in `Notes/Inbox/`.

**Key design choices:**
- **Quality Gate runs before storage.** Items below 3/4 default to `_Queue/`. The gate is the single most opinionated piece of the system.
- **Three-zone format.** Zone A verbatim (>=40%), Zone B metadata, Zone C decomposed SOP (conditional).
- **Dispatch plan first.** Agent proposes; user approves; agent writes. Never auto-files.
- **Atomic moves.** Source is moved to `_archive/ingested/` only after destination is successfully written.

Full skill: [.claude/skills/ingest/SKILL.md](../../.claude/skills/ingest/SKILL.md)

## 2. Query (`/wiki <q>`)

**Purpose:** Search the wiki, synthesize an answer, cite every claim, surface missing cross-links.

**Triggered by:** `/wiki <question>`, "what do I know about X", any open question.

**Key design choices:**
- **Cite every claim** with `[[wikilinks]]`. The user must be able to verify.
- **Bump `last-retrieved`** on every cited page. Anchors the Node Decay lint check.
- **Surface missing cross-links** the user can approve and patch. Querying improves the wiki.
- **Flag low-reliability citations** explicitly.

Full skill: [.claude/skills/wiki/SKILL.md](../../.claude/skills/wiki/SKILL.md)

## 3. Lint

**Purpose:** Health-check one layer at a time. Find decay before it spreads.

**Triggered by:** `/lint <layer>`, "lint my wiki", "health check".

**Seven checks:**
1. **Orphans** - pages with zero inbound wikilinks
2. **Stale** - `last-updated` >90 days
3. **Contradictions** - cross-page conflicts (drops in-page callouts)
4. **Broken links** - wikilinks to nonexistent pages
5. **Node decay** - `last-retrieved` >90 days
6. **Gaps** - entities referenced >=3 times without a canonical page
7. **Zone C drift** - decomposed pages that drifted to summary mode

**Key design choices:**
- **No auto-fix.** Reports only. Human chooses.
- **One layer at a time.** All-at-once produces an unreadable report.
- **The contradiction callout exception.** This is the one write-action: drop `[!contradiction]` callouts directly into conflicting pages, because contradictions are most useful when surfaced in-context.

Full skill: [.claude/skills/lint/SKILL.md](../../.claude/skills/lint/SKILL.md)

## Why three, not five

A first draft had five operations: ingest, query, lint, promote, retire. Promote got pulled into its own skill (it requires fresh-session validation, which warranted a separate doc). Retire was folded into lint - the "what to archive" decision happens by inspecting lint output, not by a separate operation.

Three operations is the minimum that covers the lifecycle. Anything more and you're naming sub-steps, not operations.

## Operation ordering

In a typical week:
- **Daily-ish:** `/ingest` (when you have new material)
- **Weekly:** `/queue-review` (items about to expire)
- **Weekly-ish:** `/wiki <q>` (whenever you have a question)
- **Monthly:** `/lint <layer>` (one layer per week, rotating)
- **As-needed:** `/queue-triage` (when queue swells), `/promote` (in fresh session)
- **At session end:** `/hot-cache` (refresh recent context)
