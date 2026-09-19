---
name: writing-claude-md-and-skills
description: What belongs in a CLAUDE.md versus a skill, how to write both effectively for Claude as the reader, and how to scope a skill — for home (`~/.claude`) or any other project. TRIGGER - about to create or edit any CLAUDE.md file, or create/modify any skill.
---

## Placement: CLAUDE.md vs. a Skill

A CLAUDE.md loads unconditionally within its scope — home's on every session, a project's whenever that repo is opened. A skill loads only when the model judges its trigger to match the task at hand.

Put a fact in CLAUDE.md when either of these applies, and its importance (below) is worth carrying every session:

- Loading it on demand would come too late — a guardrail against an action that may already have happened by the time a trigger fires (e.g. don't stage a file without permission).
- Loading it on demand would save nothing — a baseline orientation fact almost every task in scope needs anyway (e.g. the repo's purpose, or where a given concern's config lives).

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

Write for Claude specifically, not for a human skimming once.
Why: Claude re-reads the whole file fresh every session rather than recalling a prior skim, so restating a point across several sentences spends tokens without resolving any ambiguity, and buries the rule under the qualification around it.

Don't restate a fact Claude already reliably knows — a tool's default behavior, a standard convention, common practice.
Why: these files exist to close the gap between what Claude already knows and what's specific to this repo or user, not to re-teach generic knowledge.
State it only when this repo or user diverges from that default, or when it's a fact Claude can't be trusted to have right — past its knowledge cutoff, obscure or low-adoption, or a private/internal thing that collides in name with something public.

Don't restate a fact already declared canonically somewhere else — `package.json`, a config file, another skill. Point at the source instead.
Why: duplication drifts out of sync the moment the canonical source changes.

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
- It carries a concern with no home equivalent: orienting a fresh session to a repo it has never seen. Lead with the repo's purpose (the one fact no source file states directly, and README doesn't substitute for it — don't assume it'll get read); note stack/language only when it isn't obvious from the file layout; state where each concern's config actually lives (can't be reliably guessed); surface load-bearing gotchas, ordered by importance (above).

## Grouping Skills by Trigger

Group skills by how they actually get triggered, not by topic alone.

If two pieces of guidance would always fire together, keep them in one skill.
Why: splitting doesn't reduce the context loaded, since the same total content loads either time, and risks the model invoking one and forgetting the other.

If a skill's content would rarely all be relevant at once, split it.
Why: a skill's one-line description sits in the always-injected skill listing whether or not it's ever invoked, so an overly broad skill both wastes context when loaded for only part of its content and makes the listing itself heavier.

Judge "would always fire together" by the actual content, not by how similar or different the triggers sound:

- `lefthook-facts` and `github-actions-facts` sound like the same topic ("editing a YAML CI config") but share no real content, so they're correctly kept separate.
- This skill is the mirror case: "editing home CLAUDE.md" and "editing a project's CLAUDE.md" sound like different triggers, but once decomposed, most of the content — the placement test, the importance definition, and the writing principle above — is identical between them. Only the "Home vs. Project" section above is genuinely scope-specific, so they're merged into one skill instead of staying split.

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

## Skill Loading Isn't Guaranteed

Skill loading is a judgment call the model makes each turn from the listing, not something the harness guarantees — so a well-placed skill can still go unloaded in a given session. This is a real cost to weigh in the placement test above, not a reason to preemptively add reinforcement everywhere; only add an explicit pointer for a specific case once it's actually observed failing in practice.
