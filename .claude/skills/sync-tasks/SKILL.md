---
name: sync-tasks
description: "Detect drift between Personal OS (MEMORY.md, hot.md, CLAUDE.md) and the user's external scheduled tasks. Surface stale references, missing project context, and scope misalignment. Propose patches for approval - never auto-applies. Triggers: /sync-tasks, sync my tasks, are my tasks up to date, task drift check."
last-updated: "2026-05-17"
---

# Sync Tasks

Detect drift between what the Personal OS *thinks* the user is working on (`MEMORY.md`, `hot.md`, `CLAUDE.md` Routing Map) and what their *scheduled tasks* actually reference. Surface the gaps; propose patches; do not auto-apply.

## When to use

User says `/sync-tasks`, "sync my tasks", "are my tasks up to date", "task drift check", or runs a periodic maintenance cadence (typically every 2 weeks).

## Step 1 - Inventory current state

Read:
- `MEMORY.md` Active Projects
- `00_Resources/hot.md` ("What I'm working on right now" + "Up next")
- `CLAUDE.md` Routing Map (list of workstations)
- All scheduled tasks the user has set up (via `list_scheduled_tasks` or equivalent)

## Step 2 - Detect drift

For each scheduled task, check:

### Drift A - Stale entity references
The task prompt references an entity (project, person, tool) that no longer appears in `MEMORY.md` or `hot.md`. Likely stale.

### Drift B - Missing project context
The user added a project to `MEMORY.md` Active Projects but doesn't have any scheduled task related to it. Suggest one.

### Drift C - Scope misalignment
The task is doing something outside the user's declared Wiki Scope (in `CLAUDE.md`). Either the scope or the task needs to change.

### Drift D - Frequency mismatch
The task runs daily but the underlying need is weekly (or vice versa). The hint: the user keeps saying "this is too frequent" or "I want this more often" in chat history.

### Drift E - Stale paths
The task prompt references a folder path that no longer exists in the workspace. Likely a renamed workstation or restructured layer.

## Step 3 - Propose patches

For each drift detected, render a proposed patch:

```
## Drift: <task-id> - <task-title>

**Detected:** <which drift type>
**Evidence:** <one line of evidence from the workspace>
**Proposed patch:**

  <Either an updated prompt, a frequency change, a stop suggestion, or a "no patch - this looks intentional">
```

## Step 4 - Wait for user approval

The user approves per-task. Don't bundle.

## Step 5 - Apply approved patches

For each approved patch, update the scheduled task (or remove / create it as needed). Confirm each application.

## Failure modes

- **Auto-applying without approval.** Never. Tasks have already-running behavior, so a wrong patch causes wrong behavior at the wrong time.
- **Over-aggressive deletion.** If a task hasn't fired in a while, that's not evidence it's stale - it might just be on a rare schedule. Confirm before suggesting deletion.
- **Missing the user's actual workflow.** If the user has tasks doing things that aren't documented in `MEMORY.md`, surface them as "undocumented active workflows" and propose adding them to Active Projects rather than removing the task.
