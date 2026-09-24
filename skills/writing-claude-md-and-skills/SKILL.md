---
name: writing-claude-md-and-skills
description: What belongs in a CLAUDE.md versus a skill, how to write both effectively for Claude as the reader, and how to scope a skill — for home (`~/.claude`) or any other project. TRIGGER - about to create or edit any CLAUDE.md file, or create/modify any skill.
---

## Placement: CLAUDE.md vs. a Skill

A CLAUDE.md loads unconditionally within its scope — home's on every session, a project's whenever that repo is opened. A skill loads only when the model judges its trigger to match the task at hand.

Put a fact in CLAUDE.md when either of these applies, and its importance (below) is worth carrying every session:

- Loading it on demand would come too late — a guardrail against an action that may already have happened by the time a trigger fires (e.g. don't stage a file without permission).
- Loading it on demand would save nothing — a baseline orientation fact almost every task in scope needs anyway (e.g. the repo's purpose, or where a concern's config lives when its file name doesn't say).

Everything else belongs in a skill: useful for one specific kind of task, and cheap to miss until that task comes up.

## Importance: Frequency × Severity

Importance is how often a fact is actually relevant to a task, multiplied by how bad it is when missed. Use it to rank content — what order a list goes in, what earns space over something else — in a home or project file alike.

Rank severity, roughly:

- Hard failure
- Misleading or soft failure
- Workflow friction
- Purely cosmetic surprise

A fact on the bottom rung sorts last however often it fires, and doesn't earn always-loaded space at all — if it's worth writing down, it goes in a skill. A formatter silently reordering something is not the same class of problem as a rejected commit or a broken build, even if it happens more often.

Above that floor there's no fixed cut line for what clears the bar for CLAUDE.md — repos vary too much for one. Rank honestly, then judge.

## Writing the Content

This is on top of `prose-conventions`'s general rules for writing and editing prose; what follows is specific to CLAUDE.md and skills.

Write for Claude specifically, not for a human skimming once.
Why: Claude re-reads the whole file fresh every session rather than recalling a prior skim, so restating a point across several sentences spends tokens without resolving any ambiguity, and buries the rule under the qualification around it.

Open every CLAUDE.md with this line:

> This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A home CLAUDE.md ends it `in any repository` instead, matching its scope — home guidance holds across every repo, a project's only in its own.
Why: this line is the single exception to the rule above. It's written for a human who opens the repo without knowing what a CLAUDE.md is; Claude gains nothing from it, since the harness already labels the file when injecting it, so a pass optimizing for Claude as the reader deletes it unless told not to. It's also the only such exception — everything below the line is for Claude.

Keep the file as a whole as small as it can be while still doing its job — every line of a CLAUDE.md loads in every session in its scope, and every line of a skill each time it loads.

Don't restate a fact Claude already reliably knows — a tool's default behavior, a standard convention, common practice.
Why: these files exist to close the gap between what Claude already knows and what's specific to this repo or user, not to re-teach generic knowledge.
State it only when this repo or user diverges from that default, or when it's a fact Claude can't be trusted to have right — past its knowledge cutoff, obscure or low-adoption, or a private/internal thing that collides in name with something public.

Restate a fact already declared canonically somewhere else — `package.json`, a config file, another skill — only when nearly every task in scope needs it and it rarely changes. Otherwise, point at the source instead.
Why: a restated fact saves a lookup in every session but drifts out of sync whenever its source changes, so it pays off only when the lookup is near-universal and the drift rare. A repo's purpose clears that bar even though `package.json`'s description also states it; its dependency versions or script list don't.

State each remaining rule as a compact statement. Add one `Why:` line only when the rule requires judgment on a case the text doesn't explicitly enumerate — the rationale is what lets that judgment generalize to the new case; omit it for an arbitrary convention with no judgment call (e.g. commit message casing). Add a pair of examples side by side when a boundary is easier to see than to describe — correct vs. incorrect for what a rule says, bloated vs. compact for how it's written.

Section headers are Title Case, and the `Why:` prefix is plain, not bold.

Example — bloated:

> Never modify the staging status of any file (`git add`, `git restore --staged`, `git reset`, etc.) unless the user explicitly allows or asks for it. The user routinely stages a subset of files before a commit as a deliberate checkpoint marking which changes they've reviewed and agreed to. If a task seems to require changing what's staged or unstaged, stop and flag it instead — that's a sign something is wrong with the task or approach, not a cue to touch the staging area.

Compact:

> Never modify staging status (`git add`, `git restore --staged`, `git reset`) without explicit permission.
> Why: the user stages a subset of files as a deliberate checkpoint of what they've reviewed and accepted.
> If a task seems to need a staging change, that's a sign the task/approach is wrong — stop and flag it, don't touch staging to make it work.

## Home vs. Project

Home (`~/.claude/CLAUDE.md` and `~/.claude/skills/`) loads for one user, on every repo.

- Keep content general enough to hold in any repo, not tied to one project's specifics.
- When something headed for a project's CLAUDE.md or skill turns out to be generic — true of any repo, not just this one — it belongs in home instead, written once rather than repeated per project.

A project's CLAUDE.md loads for anyone who opens that repo, including someone with no home config at all.

- Write it to read the same and hold up for any user, on any machine — assume zero memory of any session, `~/.claude`, or other personal source.
- It carries a concern with no home equivalent: orienting a fresh session to a repo it has never seen. Below the opening line, lead with the repo's purpose (README doesn't substitute for it — don't assume it'll get read); note stack/language only when it isn't obvious from the file layout; state where a concern's config lives only when it can't be guessed from the tool's standard file name (e.g. two tsconfig files with different jobs); surface load-bearing gotchas, ordered by importance (above).
- Keep that orientation a map, not the territory: say what exists and where to look, only where a directory listing wouldn't already make it obvious, and leave how it works to the files themselves. "`src/cli/commands/` holds one yargs command per file" is map; "each command exports a `createXxxCommand()` factory taking injected streams" is territory, picked up by reading the file anyway.

## Scoping Skills by Trigger

Scope skills by how they actually get triggered, not by topic alone — whether naming one being created, or deciding whether two that already exist should merge or split.

If two pieces of guidance would always fire together, keep them in one skill.
Why: splitting doesn't reduce the context loaded, since the same total content loads either time, and risks the model invoking one and forgetting the other.

If a skill's content would rarely all be relevant at once, split it.
Why: a skill's one-line description sits in the always-injected skill listing whether or not it's ever invoked, so an overly broad skill both wastes context when loaded for only part of its content and makes the listing itself heavier.

Judge "would always fire together" by the actual content, not by how similar or different the triggers sound:

- `lefthook-facts` and `github-actions-facts` sound like the same topic ("editing a YAML CI config") but share no real content, so they're correctly kept separate.
- This skill is the mirror case: "editing home CLAUDE.md" and "editing a project's CLAUDE.md" sound like different triggers, but once decomposed, most of the content — the placement test, the importance definition, and the writing principle above — is identical between them. Only the "Home vs. Project" section above is genuinely scope-specific, so they're merged into one skill instead of staying split.

Naming a brand-new skill is the same judgment made earlier, before there's a second skill to weigh it against: scope its name to the surface its trigger covers, not to the one rule that prompted it.
Why: name and description are all that's visible before a skill loads, so a name scoped to one rule suppresses loading even behind a wide trigger, and leaves the next unrelated rule about that surface with nowhere to go but a second skill matching the same files.

Test it while naming, since there's no sibling yet to check it against: would an unrelated second rule about the same activity already fit under this name? `yaml-conventions` was nearly named `yaml-quoting-conventions` — scoped to the one rule that prompted it — before a rename caught it. The subject widens, not the content: `lefthook-facts` and `github-actions-facts` still stay separate from `yaml-conventions` despite all three firing on YAML files, since none of their content would ever fire together.

## Writing a Skill's Frontmatter

Only `name` and `description` are required by the harness. The rest below is this repo's own convention, not a standard:

```
---
name: kebab-case-name
description: One line describing what the skill covers, ending with TRIGGER - <when to load this> and, if relevant, SKIP - <a case that looks similar but shouldn't trigger it>.
---
```

Name and description are all that's visible before a skill loads, so the description's whole job is making the trigger fire when it should. Name concrete, checkable conditions — a file path, a command, a kind of task — rather than a vague topic.

When in doubt, write the trigger wide — this is about the trigger's phrasing, not the skill's scope: a wide trigger over tightly scoped content, never a grab-bag skill.
Why: a skill loading when it wasn't needed costs some context, while a skill missing when it was needed costs the guidance entirely — the worse failure, until there's evidence of over-triggering actually causing harm.

Keep `SKIP` about this skill's own content, not another skill's name. It exists to stop this skill from firing on a case that resembles the trigger but where its content would misfire — not to redirect to wherever a different topic is actually handled. Before writing one, check that the excluded case isn't already distinguished by the trigger's own wording (a different named tool, a different named action); if it is, the skill was never going to fire there and the exclusion guards against nothing. A cross-reference to another skill is a different kind of statement — see Linking Skills and CLAUDE.md below for where that belongs.

## Skill Loading Isn't Guaranteed

Skill loading is a judgment call the model makes each turn from the listing, not something the harness guarantees — so a well-placed skill can still go unloaded in a given session. This is a real cost to weigh in the placement test above, not a reason to widen every trigger indefinitely. A link is the other way to compensate for it — see Linking Skills and CLAUDE.md below for when to raise one.

## Linking Skills and CLAUDE.md

A link — CLAUDE.md pointing at a skill, or one skill pointing at another — exists to get content into context that skill loading alone might miss. Its only job is unlocking something not already there.

Point a link only at something conditionally loaded: CLAUDE.md → skill, or skill → skill. Never skill → CLAUDE.md.
Why: CLAUDE.md is already loaded unconditionally within its scope, so pointing at it from a skill restates something already in context instead of surfacing something new.

Write a link inline in the body, at the specific point that needs it — never in the frontmatter `description` or `SKIP`.
Why: `description` and `SKIP` exist to decide whether to load the skill at all; a cross-reference answers a different question — what to also read once it's loaded — and folding the two together makes the trigger harder to judge at a glance.

Scope a link to the specific sub-action that needs it, not the whole trigger surface it's attached to.
Example: a skill covering both editing and reviewing a file may only need a linked skill's content for the editing case — write the link against that case, not the skill as a whole.

Propose a link whenever a skill or CLAUDE.md is being written, created, or updated: look at what the change touches and surface any other skill whose content plausibly applies. Proposing isn't adding — it still waits for explicit approval like any other edit — so it doesn't need a miss to have already happened; noticing the possibility is reason enough to raise it.
Why: proposing costs nothing but a suggestion, while a skill silently failing to load costs the guidance entirely — so the bar for raising a link should be much lower than the bar for keeping one.
