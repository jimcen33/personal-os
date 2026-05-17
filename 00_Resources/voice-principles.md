---
last-updated: 2026-05-17
---

# Voice Principles

This file documents your writing voice. Cowork reads it before drafting anything on your behalf.

**Easiest way to fill this in:** run `/voice-extract` after pasting 5–10 of your recent sent emails, posts, or DMs. The skill will detect patterns and write this file for you. Then you edit.

---

## Identity

> One sentence on who you are and what you write about. Cowork uses this for tone calibration.

_(e.g., "I'm a solo founder building [thing] for [audience]. I write about [topics] with a [voice descriptor — wry / direct / curious / etc.] register.")_

---

## Voice Patterns (filled in by `/voice-extract` or by hand)

### Sentence shape
- _(e.g., "Short, declarative. Sentences average 12–18 words. One idea per sentence.")_

### Word choices
- **Use:** _(e.g., specific verbs, technical precision, plain English)_
- **Avoid:** _(e.g., "leverage," "synergy," "robust," any business-school cliché)_

### Punctuation
- _(e.g., "Em dashes for asides. Avoid semicolons unless splitting a list. No exclamation marks except in deliberate context.")_

### Structural preferences
- _(e.g., "Open with the conclusion. One paragraph per idea. Bullets only when items are truly parallel.")_

### Banned phrases
- _(List phrases you never want Cowork to use. Examples: "I hope this finds you well", "let's circle back", "deep dive", "moving forward")_

---

## Format Rules

- Default output is Markdown (.md).
- Use bullet points for lists, paragraphs for explanations.
- No emoji unless the topic explicitly calls for it.
- Headers in sentence case, not Title Case.

---

## Workstation Overrides

Specific workstations may layer additional voice rules. Cowork loads workstation `CLAUDE.md` AFTER this file, so workstation rules win on conflict.

- **Writing HQ** — long-form, narrative-driven. See `Writing/Writing HQ/CLAUDE.md`.
- **Email HQ** — match the formality of the message you're replying to. See `Email HQ/CLAUDE.md`.

---

## How to update this file

- After `/voice-extract`, review what the skill wrote. Edit.
- When you catch Cowork producing tone-off output, add a banned phrase or pattern here.
- Run `/voice-extract` every 3–6 months to recalibrate against your most recent writing.
