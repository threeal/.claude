---
name: writing-project-claude-md
description: What to include and exclude when writing or updating a CLAUDE.md for a project other than ~/.claude. TRIGGER - about to create or edit a CLAUDE.md file in any repository other than ~/.claude itself. SKIP - editing ~/.claude/CLAUDE.md or ~/.claude/skills/ (see claude-home-conventions for that).
---

A project's CLAUDE.md should work as a competent baseline for starting on anything in the repo — not a deep reference for any one task. Assume Claude already has generic engineering knowledge and needs only what's specific to this repo.

## What to include

- Skip generic tool descriptions entirely (e.g. "Linter configured in `eslint.config.ts`," "Package manager") — they restate what Claude already knows and add nothing.
- The opening description is the first thing read to orient on a repo never seen before, and README.md doesn't substitute for it — it's written for a human skimming once, not re-read fact-dense every session, so don't assume it'll get read. Lead with the repo's purpose (what it does, why it exists): that's the one fact no source file states directly. Add language/stack/artifact type only when it isn't already obvious from the repo's file layout, and note what's deliberately absent or placeholder for a scaffold/template repo.
- Do state where a given concern's configuration actually lives, since that can't be reliably guessed (an ESLint config alone could be `.js`, `.ts`, `.mjs`, or `.cjs`) — cheaper to say once than have it rediscovered by search every session.
- Devote the real space to facts that are genuinely non-obvious and load-bearing: the kind that are easy to get wrong and expensive to debug (a config split across two files by purpose, an import-path rule tied to a compiler setting, a coverage threshold that behaves unexpectedly across a whole run instead of per file, a formatter plugin silently changing default behavior, a pre-commit hook that can fail and need a re-stage-and-retry). Keep these together in their own section — don't bury them inside generic tool descriptions or split them off into a section disconnected from the tool they belong to.
- Don't restate a fact that's already declared canonically elsewhere in the repo (e.g. output paths already in `package.json`) — point at the source instead, since duplication drifts out of sync.
- Organize by importance, not alphabetically by tool or file name: weigh how often a fact is actually relevant to a task against how bad it is when missed, from a hard failure at the top down through a misleading/soft failure, then workflow friction, then a purely cosmetic surprise. A fact with zero real consequence sorts last regardless of how often it fires — a formatter silently reordering something isn't the same class of problem as a rejected commit or a broken build, even if it happens more often.
- Keep the whole file small: bloat both buries what matters and creates upkeep debt, since prose restating a config goes stale the moment that config changes.

## How to write it

Before writing, mentally set aside anything known only from this session, from `~/.claude`, or from any other personal/global source — write as if starting fresh, with no memory of who's asking or what machine/session this is. A project's CLAUDE.md must read the same and hold up for any new session, on any machine, for any user working in that repo — not just the current one.

Prefer dense fragments over full sentences throughout — drop filler like "This is a" or "There is no," and say a fact in as few words as carry it: CLAUDE.md is read by Claude every session, not skimmed once by a human, so token-per-fact density matters more than prose flow. This is why the rest of a good CLAUDE.md already reads as fragments — bullets, `Config map`, `Commands` — and the opening description should read the same way.

When editing an existing CLAUDE.md rather than writing one from scratch, the same rework-rather-than-append approach as any other prose applies (see updating-existing-prose), plus two things specific to CLAUDE.md:

- When pruning or condensing, verify nothing load-bearing was lost by checking the actual underlying config/source files directly, not just by comparing the new prose to the old prose — a rewrite can look internally consistent while having silently dropped a real fact.
- Keep the result as self-contained as the "write it fresh" rule above requires — a rewrite can quietly absorb assumptions specific to the current session, machine, or user that the original avoided.
