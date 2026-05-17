---
last-updated: 2026-05-17
---

# Writing HQ — Workstation Rules

## Identity

Writing HQ is where long-form output lives: video scripts, articles, newsletters, essays, marketing copy. Drafting, voice-checking, humanizing, and assembling final pieces happen here. Quick notes, raw thoughts, and pre-decision brainstorming go to `Notes/Inbox/` instead.

## Resources

| Resource | Read when... |
|---|---|
| `00_Resources/voice-principles.md` | Before drafting any new content |
| `Writing/Knowledge/` | Looking for writing-craft methodology |
| `Writing HQ Resources/` | Templates, brief seeds, persona references for this workstation |

## Workflow (9 steps — STOP gates marked)

This is the canonical drafting workflow. **Do not skip steps. Do not collapse steps.** STOP gates require explicit user approval before continuing.

1. **Confirm format.** What is this? (Article / newsletter / video script / X thread / LinkedIn post / other.) **STOP** — wait for confirmation.
2. **Brainstorm.** Read voice-principles.md. Generate 8–12 angle options with one-line pitches. **STOP** — user picks one or combines.
3. **Outline.** Build a section-by-section outline (headers + one-line pitch per section). **STOP** — user approves structure.
4. **SEO Lock.** If the piece has SEO intent: lock in target keyword(s), title, slug, meta description. Skip this step for non-SEO content. **STOP** if SEO is in scope.
5. **Draft.** Write the full piece. Length per the format spec. No padding.
6. **Voice pass.** Re-read against voice-principles.md. Fix mismatches.
7. **Humanizer pass.** Run the `humanizer` skill (or its prompt) over the draft. Remove AI-writing tells: em-dash overuse, "rule of three," inflated symbolism, vague attributions, promotional language.
8. **Image prompts (if applicable).** Generate prompts for hero / inline / social images. Skip if no images needed.
9. **Show together.** Present final draft + voice notes + humanizer changes + image prompts as one bundle. **STOP** — user reviews and edits or approves.

## Editorial Rules

Follow my voice principles in `00_Resources/voice-principles.md`. The rules below layer on top:

- **Open with the conclusion.** Long-form readers leave fast — give them the punchline in paragraph 1.
- **One paragraph per idea.** No multi-claim paragraphs.
- **No corporate-speak.** Words to avoid include: leverage, synergy, robust, holistic, paradigm, ecosystem (when used non-literally).
- **Show the trade-off.** If you make a recommendation, name what it costs.
- **Specific over abstract.** A real example beats three abstractions.
- **Cite when claiming.** Numbers, statistics, attributed quotes need a source.

## Failure modes to catch

- **Padding.** If a sentence doesn't move the reader forward, cut it.
- **Hedge stack.** "Could," "might," "perhaps," "in some cases" piled up = nothing said. Pick one and commit.
- **Rule of three.** Triplet noun-stacks ("clarity, conviction, and commitment") are an AI tell. Use two or four, not three, unless three is genuinely the right count.
- **Generic intro.** Skip throat-clearing intros. Start in the middle of the action.

## When to skip the workflow

For very short pieces (<200 words), collapse Steps 2–4 into a single "draft + check" pass. Steps 5–9 still apply.

For pieces that started as a `Notes/_Queue/` brief seed, treat the brief seed as Step 2's output and start at Step 3.
