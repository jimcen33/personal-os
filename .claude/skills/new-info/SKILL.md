---
name: new-info
description: "Real-time UI and info verifier. Searches the web for current accurate information - especially app UI workflows, system settings, and fast-moving topics. Cross-verifies 2+ sources. Refuses to hallucinate UI paths or outdated facts. Triggers: /new-info, what's the latest on X, current state of Y, how does Z work right now."
last-updated: "2026-05-17"
---

# New-Info (Real-Time Verifier)

Get the *current* answer, not the training-data answer. Built for app UI changes, system settings, and fast-moving topics where being wrong is worse than being slow.

## When to use

User says `/new-info <topic>`, "what's the latest on X", "current state of Y", "how does Z work right now", or asks anything that depends on recent UI / API / policy state.

Examples that route here:
- "How do I change my privacy settings in <app>?"
- "What's the current pricing for <tool>?"
- "Is <feature> still available in <tool>?"
- "What's the latest version of <library> and what changed?"

## Hard rules

- **Refuse to hallucinate UI paths.** If the search doesn't return a confirmed answer with a recent date, return "unable to verify - here's what I found, but you should double-check."
- **Cross-verify with 2+ sources** before reporting as confirmed.
- **Date every claim.** Each fact gets a "as of <date>" tag.
- **Prefer official docs over blog posts.** Especially for UI / API / pricing.
- **Surface conflicts.** If two sources disagree, name both. Don't pick a winner.

## Step 1 - Search

Run 2-4 searches optimized for currency:
- "<topic> 2026" (or current year)
- "<topic> changelog" / "<topic> release notes"
- Official documentation URL guess
- "<topic> reddit" or "<topic> community" for crowd-confirmed UI paths

Read the top results. Note the publication date of each.

## Step 2 - Filter for currency

Discard sources older than 12 months for fast-moving topics (UI, pricing, APIs). For slower-moving topics (concept explanations), 24 months is OK.

If all sources are stale, return "no recent confirmed answer found - here's the most recent I have."

## Step 3 - Cross-verify

For each claim worth verifying:
- Find 2+ independent sources that confirm it
- If only 1 source: tag the claim as "single-source"
- If sources conflict: report both with their dates

## Step 4 - Render the answer

Format:

```
## <Topic> - as of <YYYY-MM-DD>

**Confirmed:** <claim> [source-1, source-2]
**Confirmed:** <claim> [source-1, source-2]

**Single-source:** <claim> [source-1] (treat as provisional)

**Conflicting:** <source-1 says X; source-2 says Y> (you'll need to decide)

**Couldn't verify:** <thing the user asked about that the search didn't surface>
```

## Step 5 - Suggest a save action

If the answer is durable (the user will need to retrieve this again), suggest saving it to `Notes/Inbox/` for ingest. If transient (a one-time lookup), suggest no save.

## Failure modes

- **Hallucinating UI paths from training data.** Don't. If the search didn't return it, say so.
- **Trusting a single blog post.** Pricing, API behavior, and UI paths change. A 2024 blog post claiming "X is now in the Settings menu" may be stale.
- **Conflating product names.** Some queries return results for a similarly-named product. Confirm the answer is about the specific product the user asked about.
- **Date-blindness.** If the search engine doesn't surface publication dates, look at the page itself or the URL slug. If no date is detectable, treat as "undated, low confidence."
