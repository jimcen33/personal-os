---
name: voice-extract
description: "Extract the user's writing style from sent emails or pasted samples. Updates 00_Resources/voice-principles.md with detected patterns: sentence shape, word choices, punctuation, structural preferences, banned phrases. Triggers: /voice-extract, refresh my voice, extract my voice from emails, update voice principles."
last-updated: "2026-05-17"
---

# Voice Extract

Pull writing patterns from the user's actual writing samples and update `00_Resources/voice-principles.md`. Run on initial setup and every 3-6 months thereafter.

## When to use

- First-time setup: user runs `/voice-extract` and provides 5-10 writing samples.
- Periodic refresh: user wants to recalibrate against their most recent writing.
- After a major voice shift: user changed audiences, started a new publication, or feels their current voice is drifting.

## Step 1 - Gather samples

Ask the user for 5-10 samples of their writing. Best sources:
- Recently sent emails (3-5 representative ones)
- Recent published posts, newsletters, threads
- A LinkedIn or X post they're proud of
- A Slack/DM thread where they were articulate

Don't analyze 1 sample. Don't analyze 50 - signal dilutes.

## Step 2 - Detect patterns

For each dimension, extract observed patterns from the samples:

### Sentence shape
- Average length (in words)
- Distribution: short / medium / long
- Common structures (declarative vs. interrogative; subject-verb opener vs. clausal opener)
- Use of em-dashes, semicolons, parentheticals

### Word choices
- High-frequency content words (the user's actual vocabulary)
- Verbs they reach for vs. avoid
- Adjective density
- Industry jargon level

### Punctuation
- Em-dash use (frequency, position)
- Semicolon use
- Exclamation marks
- Quotation style

### Structural preferences
- Openings (do they bury the lede or open with the conclusion?)
- Paragraph length
- Bullets vs. prose
- Sign-offs and closers

### Banned phrases
Detect phrases the user *never* uses where the average AI assistant *would* use them. These become explicit bans.

## Step 3 - Compare to current voice-principles.md

Read the existing file. Detect drift between what's documented and what the samples show. Flag mismatches.

## Step 4 - Propose updates

Render a side-by-side:

```
| Dimension | Currently documented | Observed in samples | Proposed update |
|---|---|---|---|
```

**STOP. Wait for user approval (per-dimension or all).**

## Step 5 - Update voice-principles.md

On approval, rewrite the relevant sections of `00_Resources/voice-principles.md` preserving the file structure. Bump `last-updated` to today.

## Step 6 - Log

Append to `00_Resources/log.md`:
```
YYYY-MM-DD | voice-extract | <N> samples | <one-line summary of changes>
```

## Failure modes

- **Too few samples.** Refuse to extract with fewer than 5 samples. Tone signals need enough data to separate from noise.
- **Mixed contexts.** If samples are from radically different contexts (one is a cold email, one is a wedding toast), warn the user that voice may differ by context and ask whether to extract a "default" voice or a context-specific one.
- **Over-fitting to recency.** If the samples are all from the same week, the extracted voice is a snapshot of that week's mood. Suggest pulling samples spread across 1-3 months.
- **Conflating "wants to write like" vs. "actually writes like".** Voice principles are *descriptive*, not aspirational. If the user wants to write differently than they actually write, that's a separate piece of work.
