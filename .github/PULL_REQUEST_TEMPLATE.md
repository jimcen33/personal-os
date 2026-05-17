## What

One sentence: what does this PR change?

## Why

The problem this solves, or the improvement it makes.

## Files touched

- `path/to/file-1` - what changed
- `path/to/file-2` - what changed

## Type

- [ ] Bug or unclear-doc fix
- [ ] New skill
- [ ] New workstation pattern
- [ ] New docs page
- [ ] Translation
- [ ] Refactor of existing material (no behavior change)
- [ ] Adapter for a non-Claude client

## Checklist

- [ ] If a skill changed, the `description` field is still <= 1024 chars
- [ ] If `CLAUDE.md` / `MEMORY.md` / `AGENTS.md` / a skill / a workstation `CLAUDE.md` changed, `last-updated` is bumped to today
- [ ] If a new skill was added, it's listed in:
  - [ ] Root `CLAUDE.md` skill table
  - [ ] `docs/skills/README.md`
- [ ] If a new workstation was added, it's listed in:
  - [ ] Root `CLAUDE.md` Routing Map
  - [ ] `00_Resources/index.md`
- [ ] CHANGELOG.md updated under `[Unreleased]`

## Scope check

This PR (check one):

- [ ] **Sharpens** an existing opinionated rule
- [ ] **Adds** a new capability without removing opinionation
- [ ] **Generalizes** for non-Claude clients
- [ ] **Removes** opinionation - if so, explain why in the description

## Tested how

- [ ] Loaded the changes in a fresh Cowork session and confirmed the skill / workstation routes correctly
- [ ] Ran the skill end-to-end and the outputs are as described
- [ ] Read through the docs page as if seeing the system for the first time
- [ ] N/A (docs-only or readme-only change)

## Related issues

Fixes #X, references #Y
