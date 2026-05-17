# The Quality Gate

## The 4 questions

Before any item is filed into a Permanent layer, it must answer:

1. Will this still matter in 1 year?
2. Did it change my thinking, or just inform me?
3. Is it relevant to my declared scope or current Active Projects?
4. Will I realistically retrieve this later?

Score: 0 or 1 per question. Threshold for Permanent: 3 (provisional) or 4 (high-confidence).

Items below 3 route to `Notes/_Queue/` with a 30-day expiry.

## Why these four questions

Each captures a different failure mode of personal wikis.

### Q1 - "Still matter in 1 year"

The recency bias filter. Most things that feel important right now won't matter in 12 months. This question forces the agent (and you) to ask: "is this evergreen, or am I just emotionally close to it because it's recent?"

**Common failures:** funding announcements, model releases, news of the moment, op-eds.
**Common passes:** principles, mental models, decision frameworks, durable tools.

### Q2 - "Changed thinking, or just informed"

The novelty filter. A page that just restated something you already knew adds noise; a page that updated your model adds signal.

**Common failures:** second-brain explainers when you already run a second brain, productivity hot takes you've seen 50 versions of, "X is the future" posts that match your existing view.
**Common passes:** counterintuitive findings, well-argued cases against your prior, methodology you now use.

### Q3 - "Relevant to scope or Active Projects"

The scope discipline filter. The wiki has a declared scope (`CLAUDE.md` Wiki Scope section). Material outside scope is noise no matter how interesting.

**Common failures:** "interesting" articles in adjacent fields you don't actually work in, hobbyist deep-dives.
**Common passes:** material directly applicable to your declared scope or current Active Projects in `MEMORY.md`.

### Q4 - "Realistically retrieve later"

The retrieval-cost filter. A page only earns its keep if you'd actually search for it. Saved-and-forgotten content costs more than it returns.

**Common failures:** "general background reading," reference material covered by a quick web search, deep niche content you'll never re-look-up.
**Common passes:** specific methodologies you'll consult when running a workflow, contact details, decision rationales, your own original thinking.

## Why 4, not 3 or 5

3 questions miss either retrieval-cost or scope discipline - both of which are crucial.

5 questions creates analysis paralysis. By the 5th question the agent is splitting hairs.

4 is the smallest set that covers (a) recency, (b) novelty, (c) scope, (d) retrieval. Each is independently necessary; together they're sufficient.

## Why threshold = 3, not 4

A 4/4 requirement is strict enough that ingest stalls on borderline cases. 3/4 captures items that fail *one* dimension but succeed on the others - typically failing Q1 ("might matter in 1 year, but the recency is what's drawing me to it") or Q3 ("not perfectly on scope, but adjacent enough").

These borderline items go to `_Queue/` for 30 days. If they prove useful in that window, they get promoted; if not, they expire.

## Tuning the gate

Some users will want stricter (4/4 only); some will want looser (2/4 to Permanent). To tune:

1. Edit the threshold in `CLAUDE.md` Quality Gate section.
2. Update the ingest skill's Step 5 logic.
3. Run `/lint` after 3 months to see how the new threshold is shaping the wiki.

The default 3/4 is what works for most solo founders. Tighten if you find yourself filing too much; loosen if your queue is bloated with passes-3-of-4 items.

## When the gate fails

Failure mode #1: **The user wants to save something the gate refuses.** Solution: user says "save this anyway" - the item routes to `_Queue/` with 30-day expiry. If it's still useful in 30 days, promote.

Failure mode #2: **The gate passes everything.** Symptom of gate-question drift. The agent has loosened its interpretation. Re-anchor by re-reading `CLAUDE.md` Wiki Scope and re-running ingest with strict scoring.

Failure mode #3: **The gate refuses things that are valuable.** Symptom of scope drift in the wiki itself. You may have a new active project that hasn't been added to `MEMORY.md`. Update `MEMORY.md` first; re-ingest.
