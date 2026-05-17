# Why one folder per skill

Each skill lives in its own folder: `.claude/skills/<name>/SKILL.md`. Not `.claude/skills/<name>.md` (single file) and not all-skills-in-one-file.

This is deliberate.

## Reasons

### 1. Skills need attachments
Some skills need template files, reference data, or sample inputs. A folder lets you co-locate `SKILL.md` + `template.md` + `sample.json` + a `README.md` for contributors.

### 2. Skill versioning
The `last-updated` and `version` frontmatter fields work better when each skill is a self-contained unit. If 14 skills share one file, bumping one skill's version is messy.

### 3. Skill ownership
In a multi-contributor world, "owns the ingest skill" maps to "owns `.claude/skills/ingest/`." Cleaner than "owns lines 42-218 of skills.md."

### 4. Future plugin packaging
If you ever want to extract a skill into a plugin or a marketplace, having it in a folder makes the extraction one `mv` away.

## What goes in a skill folder

Required:
- `SKILL.md` - the skill definition

Optional:
- `template.md` - a reference template the skill uses
- `examples/` - sample inputs and outputs
- `README.md` - notes for contributors (not loaded by the agent)
- `tests/` - if you write skill-evaluation suites

## What does NOT go in a skill folder

- User data
- Generated outputs
- Logs
- Anything that changes per-session

Those go in the appropriate layer of the wiki, not in the skill folder.
