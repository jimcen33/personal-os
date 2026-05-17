---
last-updated: 2026-05-17
current-threshold: 7.0
last-calibrated: 2026-05-17
---

# Spotlight Log

Append-only log of every ROI spotlight emitted across sessions. Used to calibrate the threshold at 30 entries.

Format:

```
| date | session-id | score | leverage | specific | surprise | line | outcome |
|---|---|---|---|---|---|---|---|
| YYYY-MM-DD | <session> | X.X | N | N | N | <≤25-word line> | hit | miss | unmarked |
```

---

## Entries

| date | session | score | lvg | spec | surp | line | outcome |
|---|---|---|---|---|---|---|---|

_(empty — first spotlight will land here)_

---

## Calibration history

| date | window-size | hits | misses | unmarked | hit-rate | new-threshold |
|---|---|---|---|---|---|---|

_(empty — first calibration after 30 entries)_
