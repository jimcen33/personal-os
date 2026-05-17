---
name: tutorial-generator
description: "Convert a Claude or AI automation technique into a polished, non-engineer-friendly step-by-step tutorial in Markdown. Researches current steps via /new-info, scans the wiki for overlap, scores priority on a 1-10 + tier scale, writes a Verdict block at the top, saves to Tutorials/. Triggers: /tutorial, make a tutorial about X, turn this into a tutorial."
last-updated: "2026-05-17"
---

# Tutorial Generator

Turn a technique or workflow into a Markdown tutorial the user's non-engineer self could follow at 10pm with no caffeine.

## When to use

User says `/tutorial <topic>`, "make a tutorial about X", "turn this into a tutorial", or wants to document a useful automation tip for their `Tutorials/` workstation.

## Hard rules

- **Every UI claim must come from `/new-info` or a manually verified screenshot.** No training-data UI hallucinations.
- **Every tutorial leads with a Verdict block.** No Verdict = not ready.
- **Target reader is non-technical.** Plain English over jargon. First use of any technical term gets a one-line definition.
- **No terminal unless required.** GUI paths preferred when both exist.

## Step 1 - Run the Verdict scan

Compare the topic against existing material in the wiki:
- Root `CLAUDE.md`, `MEMORY.md`, `00_Resources/hot.md`
- `Software/`, `Knowledge/`, `.claude/skills/`
- Existing `Tutorials/_index.md`

Is this duplicate? Strictly better? A subset? An adjacent technique?

## Step 2 - Score on the 3-factor rubric

The same rubric as Spotlight Rule:
- **Impact** (1-10) - how much value if the user adopts this?
- **Novelty** (1-10) - how different from what they already do?
- **Effort** (1-10, inverted) - how easy to implement? (10 = easy, 1 = hard)

Score = (Impact * 0.4) + (Novelty * 0.3) + (Effort_inverted * 0.3)

Tier:
- Critical (>=8.0)
- High (7.0-7.9)
- Medium (5.5-6.9)
- Low (<5.5)

**Auto-cap at Low when Novelty <= 2.** A tutorial about something the user already does isn't worth writing for themselves, though it may still be worth writing for the public.

## Step 3 - Research current steps

Run `/new-info <topic>` to fetch verified-current UI paths, settings names, version numbers. Don't proceed without this.

## Step 4 - Draft using TUTORIAL_TEMPLATE.md

Open `Tutorials/TUTORIAL_TEMPLATE.md`. Follow the structure exactly:

1. **Frontmatter** - filled in
2. **Verdict & Comparison block** at the top
3. **What this tutorial does** - one-sentence outcome
4. **Who this is for** - reader profile, prior knowledge, time
5. **Before you start** - tools, accounts, prerequisites
6. **Steps** - numbered, with success criteria and failure recovery per step
7. **Troubleshooting** - top 2-3 failure modes with fixes
8. **What you can do next** - adjacent capabilities
9. **Reference** - official docs, related wiki pages
10. **Notes for future maintenance** - UI version, last-verified date

## Step 5 - Save

Write to `Tutorials/<slug>.md`. Save raw research material to `Tutorials/_raw-resources/<slug>/` if any.

## Step 6 - Update index

Append a row to `Tutorials/_index.md` with the new tutorial's metadata (slug, title, tier, priority, last-updated, status: draft).

## Step 7 - Report

Tell the user:
- Tutorial saved to `<path>`
- Verdict: <tier>, priority X.X
- Recommendation: adopt / adapt / skip / reference

## Failure modes

- **Skipping the Verdict scan.** A tutorial about something the user already does duplicates effort. Always scan first.
- **Hallucinated UI.** Refuse to write a step that wasn't confirmed by `/new-info` or a screenshot.
- **Engineer voice.** "Just `cd` into the directory and run..." - if your tutorial's first instinct is a terminal command, audit whether a GUI path exists.
- **Optimizing for completeness over usability.** 47 thorough steps that cover every edge case is worse than 12 steps that get the reader from zero to working.
