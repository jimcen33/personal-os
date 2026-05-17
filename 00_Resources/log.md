---
last-updated: 2026-05-17
---

# Activity Log

Append-only timeline of significant changes to the wiki. One line per event. Cowork appends here automatically after `/ingest`, `/promote`, and major lint cleanups.

Format:

```
YYYY-MM-DD | <operation> | <page> | <layer> | decomposed: Y/N | <one-line reason>
```

---

## Log

_(empty — populates as you operate the wiki)_

---

## Archiving

When this file passes 500 lines, move entries older than 6 months to `log-archive/YYYY-Q<n>.md`. Lint will flag the file size and prompt you.
