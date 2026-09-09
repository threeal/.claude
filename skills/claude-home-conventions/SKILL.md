---
name: claude-home-conventions
description: How to decide what belongs in ~/.claude/CLAUDE.md versus a skill, and how to scope and structure a new skill. TRIGGER - about to add or edit content in ~/.claude/CLAUDE.md, or create/modify a skill under ~/.claude/skills/. SKIP - writing a CLAUDE.md for a different project (see writing-project-claude-md for that).
---

## CLAUDE.md vs. skills

`CLAUDE.md` is loaded into every session regardless of what the task is, so it should hold only guardrails: rules that must already be active before Claude acts, because they prevent something unwanted or hard to reverse (e.g. never commit or push without being told, never touch the staging area, never merge a PR, never start implementing without an explicit go-ahead).

Anything else — process detail, technique, or reference material that only matters once a specific kind of task is already underway — belongs in a skill instead, so it's loaded on demand rather than carried into every session whether or not it's relevant. If a rule reads as "when doing X, do it this way" rather than "before acting at all, never do Y," it's a skill candidate, not a CLAUDE.md candidate.

## Grouping skills by trigger

Group skills by how they actually get triggered, not by topic alone.

If two pieces of guidance would always fire together, keep them in one skill. Splitting them doesn't reduce the context loaded — the same total content loads either time — and risks the model invoking one and forgetting the other.

If a skill's content would rarely all be relevant at once, split it. A skill's one-line description sits in the always-injected skill listing whether or not it's ever invoked, so an overly broad skill both wastes context when loaded for only part of its content and makes the listing itself heavier.

Judge "would always fire together" by the actual task, not by how similarly the triggers can be worded — e.g. editing `lefthook.yaml` and editing a GitHub Actions workflow file both sound like "editing a YAML CI config" in the abstract, but the two tasks essentially never need each other's facts, so `lefthook-facts` and `github-actions-facts` are correctly kept separate.

## Writing a skill's frontmatter

Follow the format the existing skills already use:

```
---
name: kebab-case-name
description: One line describing what the skill covers, ending with TRIGGER - <when to load this> and, if relevant, SKIP - <a case that looks similar but shouldn't trigger it>.
---
```

The description is the only thing considered before the skill is loaded, so the TRIGGER clause needs to name concrete, checkable conditions (a file path, a command, a kind of task) rather than a vague topic.

## A known limitation

Skill loading is a judgment call the model makes each turn from the listing, not something the harness guarantees — so a well-placed skill can still go unloaded on a given session. `CLAUDE.md` can mitigate this somewhat by pointing at a skill directly for cases that matter, but that's still the model choosing to follow a pointer, not a hard guarantee. Strengthening this further (e.g. with a hook) is deliberately left alone unless it's actually observed failing in practice.
