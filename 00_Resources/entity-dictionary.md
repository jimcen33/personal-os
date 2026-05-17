---
last-updated: 2026-05-17
---

# Entity Dictionary

The canonical list of recurring entities in your wiki: concepts, projects, people, tools, methodologies. Before any new Permanent page is created, Cowork checks here to avoid duplicates.

This is the **anti-duplicate** mechanism. The single most expensive failure mode in a personal wiki is creating two pages on the same idea with slightly different names.

---

## Format

```
## <Entity Name>

- **Canonical page:** [[<page-name>]]
- **Layer:** `Knowledge/` | `Software/` | `LifeOS/` | `Writing/`
- **Type:** methodology | concept | decision | reference | decomposed
- **Aliases:** ["other names this entity goes by"]
- **First seen:** YYYY-MM-DD
- **Cross-references:** [[<page-a>]], [[<page-b>]]
```

---

## Entries

_(empty — populates as you `/ingest` and `/promote`)_

### Concepts

_(empty)_

### Projects

_(empty)_

### People

_(empty)_

### Tools / Systems

_(empty)_

### Methodologies

_(empty)_

---

## Maintenance

- **Before creating any Permanent page**, search this file for the central entity.
- **During `/promote`**, the validator checks this file.
- **Quarterly**: lint this file for stale entries (entities whose canonical page is now empty or orphaned).
