---
last-updated: 2026-05-17
---

# Tutorials — Workstation Rules

## Identity

Tutorials is where Claude / AI automation techniques get turned into step-by-step guides for non-engineers. Target reader is **always** a smart person with zero coding background — explain like they're 10 years old, never assume terminal comfort.

This workstation pairs with the `tutorial-generator` skill (slash command: `/tutorial <topic>`).

## Resources

| Resource | Read when... |
|---|---|
| `TUTORIAL_TEMPLATE.md` | Every new tutorial — this is the canonical structure |
| `_index.md` | Looking for an existing tutorial or filing a new one |
| `_raw-resources/` | Reference material for tutorials in progress |
| `00_Resources/voice-principles.md` | Before writing — voice rules apply here too |

## Workflow

### When a topic comes in

1. **Verdict scan.** Compare the topic against existing material in this Personal OS:
   - Root `CLAUDE.md`, `MEMORY.md`, `00_Resources/hot.md`
   - `Software/`, `Knowledge/`, `.claude/skills/`
   - Existing `Tutorials/`

   Determine: does this duplicate something I already have? Is it strictly better? A subset? An adjacent technique?

2. **Score on the 3-factor rubric** (the same one the Spotlight Rule uses):
   - **Impact** (1–10) — how much value if I adopt this?
   - **Novelty** (1–10) — how different is this from what I already do?
   - **Effort** (1–10, inverted) — how easy to implement?

   Weighted score = (Impact × 0.4) + (Novelty × 0.3) + (Effort_inverted × 0.3)

   Tier: Critical (≥8.0) / High (7.0–7.9) / Medium (5.5–6.9) / Low (<5.5).

   **Auto-cap at Low when Novelty ≤ 2.** If I already do this, the tutorial isn't worth writing for myself — though it may still be worth writing for the public template.

3. **Verdict block.** Lead the tutorial file with a 🧭 Verdict & Comparison block at the very top:

   ```
   ## 🧭 Verdict & Comparison

   **Priority:** <score>/10 · <tier>
   **Impact:** N · **Novelty:** N · **Effort:** N

   ### Current setup vs. this tutorial
   | What I do now | What this tutorial proposes | Why it's different |
   |---|---|---|
   | ... | ... | ... |

   ### Recommendation
   <one paragraph: adopt as-is | adapt subset | already covered, skip | reference only>

   ### Business / personal impact
   <one paragraph on what changes if I adopt>
   ```

4. **Research the latest steps.** Use `/new-info <topic>` to fetch current, accurate UI paths, settings names, and version numbers. **Refuse to ship a tutorial with hallucinated UI.**

5. **Draft using `TUTORIAL_TEMPLATE.md`.** Follow the template structure. Every step has:
   - What you click / type / paste
   - Where exactly it lives in the UI (with version caveat if relevant)
   - What you should see after
   - What to do if you don't see what's expected

6. **Save** to `Tutorials/<slug>.md` with frontmatter.

7. **Update `_index.md`** with a row for the new tutorial.

## Editorial Rules

Follow my voice principles in `00_Resources/voice-principles.md`. Tutorial-specific layered rules:

- **Plain English over jargon.** First use of any technical term gets a one-line definition.
- **No terminal unless necessary.** If a tutorial can be done in a GUI, use the GUI.
- **Screenshots welcome.** Link to image files in `_raw-resources/<tutorial-slug>/`.
- **Show what success looks like.** End each step with "you should now see X."
- **Anticipate the off-ramp.** Most readers fail at one specific step — name it and offer the fix preemptively.
- **No "easy peasy" or "it's super simple."** Patronizing. The whole point is that the reader didn't find this easy.

## Failure modes

- **Hallucinated UI paths.** Every UI claim must come from `/new-info` or a manually verified screenshot. Don't trust LLM training data for current UIs.
- **Skipped Verdict block.** If a tutorial doesn't start with the Verdict & Comparison block, it isn't ready.
- **Optimizing for completeness over usability.** A tutorial with 47 steps that "covers everything" is worse than 12 steps that get the reader from zero to working.
