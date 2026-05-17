---
last-updated: 2026-05-17
---

# Topic Index

The global topic map. Cowork reads this when you ask "what do I have on X" or when ingest needs to know where similar material is filed.

The index is **maintained by ingest** — when a new page goes into a Permanent layer, ingest appends the page link under the appropriate topic. You curate it manually only when re-organizing.

---

## Format

```
## <Topic>

- [[<page-name>]] — one-sentence pitch — `<layer>` — `last-updated: YYYY-MM-DD`
- [[<page-name>]] — ...

### Sub-topic
- [[<page-name>]] — ...
```

---

## Topics

_(empty — populates as you `/ingest`)_

---

## Maintenance

- After 30 pages: review topic clustering, merge sub-topics that are duplicates
- After 100 pages: consider splitting this file by domain (`index-knowledge.md`, `index-software.md`, etc.)
- Lint catches entities referenced ≥3× without a canonical page (the "gap" check) — those go here as `gap:` candidates
