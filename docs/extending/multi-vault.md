# Multi-Vault: Growing into a Constellation

The template ships single-vault. Real founder workflows often grow into a constellation of vaults with different sensitivity profiles and audiences. This doc explains the pattern.

## When you outgrow single vault

Symptoms:

- You have material that's mine alone (financials, hiring decisions, contracts) mixed with material that's team-collaborative (engineering decisions, product specs). Different audiences, same wiki.
- You want to publish some content from the wiki publicly, but the file system makes it hard to scope what's safe to share.
- You're using the wiki for a company AND for your personal knowledge, and the boundary between them is blurry.

When two or more of those become friction, it's time.

## The constellation pattern

Three vaults, each with a different sensitivity profile:

| Vault | Purpose | Audience |
|---|---|---|
| `personal-os/` (this template) | Your personal knowledge, voice, methodologies | You + Cowork |
| `company-wiki/` | Team-shared product, engineering, ops decisions | You + colleagues + Cowork |
| `private-wiki/` | High-sensitivity: financials, hiring, contracts, investor relations | You + (very limited) + Cowork in private-only sessions |

Each is its own git repo. Each has its own `CLAUDE.md`, `MEMORY.md`, `AGENTS.md`.

## Cross-vault routing

The Personal OS's `/ingest` skill, in cross-vault mode, can auto-route material to the right vault based on a sensitivity scan.

The pattern:

1. `Notes/Inbox/` receives material from anywhere
2. `/ingest` reads each item and runs a sensitivity scan
3. Material flagged as company-relevant -> routes to `company-wiki/Notes/Inbox/` for that vault's ingest to process
4. Material flagged as high-sensitivity -> routes to `private-wiki/Notes/Inbox/`
5. Personal material stays in `personal-os/` and proceeds through normal ingest

The sensitivity scan logic lives in `00_Resources/sensitivity-rules.md` (which this template doesn't ship - you write it for your specific contexts).

## Sandbox constraints

When Cowork runs a session in one vault but the mount also includes the others, there are practical constraints:

- **Writes inside `.git/objects/`** of mounted vaults may fail (sandbox restriction).
- **Index lock contention** can leave a stuck `.git/index.lock`.
- **`git checkout -b`** on a mounted vault can hang.

Practical rule: **do file edits from Cowork; do all git operations from your local terminal.** If Cowork tries to `git commit` on a mounted vault, you'll need to clean up `.git/index.lock` manually.

## Inbound reads (one direction only)

A session in one vault can READ from another vault (cross-vault context). It must not WRITE.

Example: a session in `company-wiki/` needs your voice principles. It reads `personal-os/00_Resources/voice-principles.md` directly. Don't copy the file to company-wiki - keep it referenced from one place.

## Outbound writes (the one documented exception)

The Personal OS's `/ingest` skill in cross-vault routing mode is the ONE documented exception that writes across vaults. It routes auto-classified material from `personal-os/Notes/Inbox/` into `company-wiki/Notes/Inbox/` or `private-wiki/Notes/Inbox/`.

Any other cross-vault write must come from a session opened in the destination vault's folder.

## Bundle delivery pattern (for infrastructure changes)

When you need to ship infrastructure changes from one vault to another (new skills, new templates, new resource files, root CLAUDE.md edits), the pattern is **bundle delivery, not direct edits**:

1. The source vault's Cowork session writes the new/changed files to a `Cowork Outputs/<change-slug>-<date>/` folder, mirroring the destination structure.
2. The bundle includes a `README.md` with apply order and verification commands.
3. You apply the bundle manually from your terminal.

This avoids the sandbox issues with git operations across mounts and gives you a chance to review before applying.

## How to set this up

1. Decide which vaults you need (Personal + Company? Personal + Company + Private?).
2. For each new vault, fork or clone `personal-os` and rename - it's the same template, just with different scope and content.
3. Edit each vault's `CLAUDE.md`:
   - Wiki Scope declaration narrows to that vault's domain
   - Add a `Companion Vaults` section listing the other vaults and read/write rules
4. Write `00_Resources/sensitivity-rules.md` for the routing logic.
5. Update `personal-os/.claude/skills/ingest/SKILL.md` to add the cross-vault routing steps.

## What stays in Personal OS

Even in a constellation:
- Your voice principles
- Your personal Knowledge layer (durable understanding regardless of org)
- Your LifeOS layer (money, health, contacts)
- Your Writing/Drafts layer for personal writing
- The hot cache, the spotlight log, the entity dictionary

Personal OS remains your *primary* vault. The others extend it; they don't replace it.

## When NOT to add more vaults

- If you're solo and don't share anything with colleagues - one vault.
- If your "team" content is one Notion doc you update monthly - that's a Notion doc, not a wiki.
- If you can't articulate why a piece of content belongs to vault A vs. vault B in one sentence - the boundary isn't clear enough yet. Stay single-vault until you can.

Premature multi-vault is more painful than late multi-vault.
