---
last-updated: 2026-06-20
---

# Taste-Sync Log

Every `/weekly-taste-sync` run appends one entry here. At month boundaries, read this log to recalibrate the patch score threshold via the **rollback rate** (how many approved patches you later reverted).

- Rollback rate **high** (you keep undoing patches) → raise the score threshold; the sync is too eager.
- Rollback rate **low** (patches stick) → the threshold is healthy, or could be lowered to catch more.

## Entry format

```
## YYYY-MM-DD
- patches-proposed: N
- patches-approved: N
- adversarial-candidate: <the one mandatory devil's-advocate patch, and the verdict>
- skeptic-pass: <summary>
- rolled-back-since-last-sync: N  (patches from prior syncs you reverted this week)
- notes: <one line>
```

---

<!-- Entries below, newest first. -->
