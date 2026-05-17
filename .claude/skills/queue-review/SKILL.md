---
name: queue-review
description: "Weekly review of Notes/_Queue/ items expiring in the next 7 days. For each: promote (in a fresh session), extend with reason, or let expire. Use when the user says /queue-review, review my queue, what's in the queue, or asks about expiring items."
last-updated: "2026-05-17"
---

# Queue Review

Weekly surfacing of items in `Notes/_Queue/` that are about to expire. Decides what to act on; does not act.

## When to use

User says `/queue-review`, "review my queue", "what's in the queue", "what's about to expire", or runs a weekly maintenance cadence.

## Step 1 - Read the rules

Read `Notes/_Queue/README.md` (if present) for the queue lifecycle - 30-day expiry, status field, extension rules.

## Step 2 - List expiring items

List every file in `Notes/_Queue/` whose `expires` frontmatter date is within the next 7 days. For each, show:

- Title
- Days until expiry
- `gate-failures` (which Quality Gate questions it failed at ingest)
- 1-sentence TL;DR pulled from the file body

## Step 3 - Ask per item

For each expiring item, ask the user to choose:

- **promote** - run the validator in a fresh session
- **extend** - add 30 days with a one-line reason added to frontmatter
- **let expire** - delete on the expiry date (move to `_archive/expired/`)

## Step 4 - Do NOT promote in-session

Promotion requires a fresh-session validator per `00_Resources/promotion-checklist.md`. If the user says "promote this one," respond: "Open a new Cowork session and run `/promote <file path>`. This skill won't promote in the current session."

## Step 5 - Execute extends and expires

- For **extend** decisions: edit the file's `expires` frontmatter to +30 days from today, append a one-line reason as a comment.
- For **let expire** decisions: the queue file will be moved to `_archive/expired/` on its expiry date by the next scheduled run. Mark it `status: expired` now.

## Outputs

- A list of items reviewed with the user's decision per item
- A list of files edited (extends)
- A list of files marked for expiry

## Failure modes

- **User says "promote all".** Don't. Promote requires a fresh-session validator one item at a time.
- **Item has no `expires` frontmatter.** Treat as a malformed queue item - surface to user, propose adding `expires: <today + 30 days>`.
