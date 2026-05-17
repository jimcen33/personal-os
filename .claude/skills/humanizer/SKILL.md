---
name: humanizer
description: "Remove signs of AI-generated writing from text. Use when editing or reviewing text to make it sound more natural and human-written. Detects and fixes: inflated symbolism, promotional language, superficial -ing analyses, vague attributions, em-dash overuse, rule of three, AI vocabulary words, passive voice, negative parallelisms, filler phrases. Triggers: /humanizer, humanize this, remove AI tells, make this sound human."
last-updated: "2026-05-17"
---

# Humanizer

Remove AI-writing tells from a draft. Operates as a single pass: read the text, list the issues, propose specific before/after edits, wait for approval, apply.

## When to use

User says `/humanizer`, "humanize this", "remove AI tells", "make this sound human", or as Step 7 of the Writing HQ 9-step workflow.

## Patterns to detect

### 1. Inflated symbolism
"In a world where..." / "Imagine a future where..." opener. Rarely earns the weight.

### 2. Promotional language
"Cutting-edge," "innovative," "revolutionary," "game-changing." Generic AI marketing-speak.

### 3. Superficial -ing analyses
"Looking at the data, we can see..." / "Considering all factors..." Sentences that gesture at analysis without doing any.

### 4. Vague attributions
"Studies show..." / "Experts say..." / "It is widely believed..." Either cite or cut.

### 5. Em-dash overuse
More than one em-dash per paragraph - especially when used for the same rhetorical move (the parenthetical aside) over and over.

### 6. Rule of three
Triplet noun-stacks: "clarity, conviction, and commitment." Three is the AI default. Two or four is often the human choice.

### 7. AI vocabulary
"Leverage," "synergy," "delve into," "navigate the complexities," "in the realm of," "tapestry," "robust," "holistic," "paradigm shift."

### 8. Passive voice
"Mistakes were made." "The decision was reached." Active voice unless passive is deliberate.

### 9. Negative parallelisms
"It's not X, but Y." "Not just A, but B." Used once, fine. Used four times in 500 words, an AI tell.

### 10. Filler phrases
"It is important to note that..." "It's worth mentioning that..." "Needless to say..." All deletable.

## Step 1 - Read the draft

Read the full text. Don't fix as you go - inventory first.

## Step 2 - Inventory issues

For each detected pattern, list:
- Pattern type
- Location (paragraph / sentence)
- Original text
- Proposed fix

## Step 3 - Propose edits

Render the inventory as a table or numbered list. **STOP. Wait for the user to approve, edit, or reject per-item.**

## Step 4 - Apply approved edits

On approval, produce the edited draft. Show a diff if helpful.

## Step 5 - Calibrate

If the user rejects an edit consistently across multiple sessions, that pattern might be part of *their voice* and should be added to `00_Resources/voice-principles.md` as an explicit allow.

## Failure modes

- **Over-humanizing.** Some AI patterns are also legitimate human patterns. Em-dashes once per paragraph is fine; the issue is repetition. Rule of three is fine when three is the right count.
- **Voice mismatch.** If the user's voice principles allow / require a pattern this skill flags (e.g., user writes in formal register that uses "leverage"), respect the principles. Voice principles override default humanizer rules.
- **Mechanical synonym replacement.** Don't swap "leverage" for "use" everywhere. The fix sometimes requires a sentence restructure, not a word swap.
