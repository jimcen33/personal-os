---
name: autoresearch
description: "Autonomous 3-round web research on a topic. Searches, synthesizes, fills gaps, files results into the wiki. Reads 00_Resources/autoresearch-program.md for source preferences and constraints. Triggers: /autoresearch, research X, do a deep dive on Y, fill a knowledge gap."
last-updated: "2026-05-17"
---

# Autoresearch

Three-round web research with gap-filling. Files results into the wiki (default destination: `Notes/_Queue/`).

## When to use

User says `/autoresearch <topic>`, "research X", "deep dive on Y", or wants to fill a knowledge gap surfaced by `/lint`.

## Hard rules

- **Read `00_Resources/autoresearch-program.md` first.** It defines source preferences, trust order, and refusal cases.
- **Refuse aggregator-only findings.** If the only available sources are listicles or "10 things you need to know" articles, return "insufficient quality sources, retry with narrower query."
- **Cross-verify high-confidence claims.** A finding moves from `reliability: medium` to `reliability: high` only with 2+ independent trusted sources.
- **No predictions.** Stick to what's known unless the user explicitly asks for forecasts.
- **No paywall scraping.** Surface paywall blockers; ask the user to provide access.

## Step 1 - Confirm the question

If the topic is vague, ask the user to narrow it. "Research X" can mean five different things. Pin down the angle before searching.

## Step 2 - Round 1 - Broad scan

Run 5-8 searches across the topic. Mix:
- One search for the canonical / authoritative source
- One search for the most-cited critique
- One search for the most recent (last 6 months) discussion
- 2-5 angle-specific searches

For each result, capture URL, source-trust level, and a one-sentence relevance note.

Synthesize: what are the 3-5 sub-questions that emerge?

## Step 3 - Round 2 - Depth on sub-questions

For each sub-question from Round 1: 2-3 high-trust sources. Read primary docs / papers / repos. Avoid aggregator summaries.

After Round 2, identify remaining gaps - questions still unanswered or where sources disagree.

## Step 4 - Round 3 - Gap-filling

Targeted searches on the remaining gaps. If still unfilled, mark them as open questions in the final deliverable - don't fabricate.

## Step 5 - Write the deliverable

Create a Markdown file in `Notes/_Queue/` with frontmatter:

```yaml
---
title: <topic>
type: research-bundle
last-updated: YYYY-MM-DD
expires: YYYY-MM-DD  # +30 days
reliability: <high|medium|low - per autoresearch-program.md scoring>
gate-score: <estimated - will be re-evaluated at promotion>
status: pending-review
tags: [research, <topic-tags>]
---
```

Body:
- **One-paragraph executive summary**
- **Key findings** (3-7 bullets with citation links)
- **Open questions** (gaps not filled by 3 rounds)
- **Source list** (with trust level per source)
- **Conflicts flagged** (where trusted sources disagreed)

## Step 6 - Suggest next action

Should the user `/promote` this to a Permanent layer? Run `/ingest` to refile? Ignore for now? Make one specific recommendation, not three options.

## Failure modes

- **Topic too broad.** Refuse and ask for narrowing. "Research AI" produces nothing useful.
- **Echo-chamber sources.** If 6 sources all cite the same primary, that's 1 source. Surface the dependency.
- **Conflict suppression.** If trusted sources disagree, capture both with citations. Don't pick a winner; let the user adjudicate.
- **Hallucinated citations.** Every URL must have been actually fetched. If a fetch failed, mark it failed - don't summarize from the title.
