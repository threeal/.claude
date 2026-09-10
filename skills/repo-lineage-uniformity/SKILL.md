---
name: repo-lineage-uniformity
description: Keep shared config, docs, and structure uniform across a hierarchy of related threeal repos derived from one another. TRIGGER - about to edit a config file, README, workflow, or other shared-style file in a repo whose GitHub remote is under github.com/threeal. SKIP - editing repo-specific application code/content that has no shared counterpart elsewhere in the graph, or a repo confirmed not part of this ecosystem.
---

Many repos under https://github.com/threeal aren't standalone — each one can derive from a parent repo, and any repo can have children of its own, regardless of whether it's itself a template. `action-starter` is a template, but so is `setup-pnpm-action` in the sense that it could gain children of its own later — being a leaf today doesn't make it structurally different. Every edge in this graph is the same kind of relationship, governed by the same rule; don't treat template-to-template and template-to-non-template edges differently.

## The Known Graph

```
project-starter -> nodejs-starter
nodejs-starter -> action-starter
nodejs-starter -> react-starter
action-starter -> setup-lefthook-action
action-starter -> setup-pnpm-action
react-starter -> greenwall
```

This list is intentionally incomplete — other threeal repos participate in this same hierarchy but haven't been placed in it yet. Extending the graph is a separate task from this skill's job: don't try to deduce a repo's position by exploring the user's account. If you're working in a repo whose GitHub remote is under `github.com/threeal` but it isn't in this list, ask the user whether it should be added (and where), or whether it simply isn't part of this system — if the latter, none of the rest of this skill applies to that repo.

## What Must Stay Uniform

Shared elements — configuration files, documentation wording/style/tone, structural ordering, and similar generic conventions — should be implemented identically between a repo and its parent, unless a specific part was deliberately chosen to be different. This doesn't extend to genuinely repo-specific content with no counterpart elsewhere (e.g. `react-starter`'s React-specific setup has no equivalent in `action-starter`).

## Default: Follow the Parent

The default, cheap path is top-down: a child adopts whatever convention already exists in its parent, checked by comparing against just the immediate parent — routine work doesn't require scanning siblings, cousins, or the rest of the graph. But this is a default, not an obligation: a child can challenge the parent instead of following it (see below), as long as the challenge is validated first.

## Challenging a Parent

Sometimes a child concludes its parent's norm is actually wrong. Before raising that, validate it by scanning outward through the graph — the parent's own parent, other ancestors, and siblings, as far as is relevant. This is a fact-check, not a propagation step: the parent can be wrong, but the child's proposed alternative can also turn out to be wrong once you see what a grandparent or a sibling already did — e.g. a grandparent may have already tried the same idea and reverted it for a reason that applies here too, or a sibling may have hit the same question and settled it differently for a reason worth knowing about. Only raise the challenge once it survives that check.

For example: `setup-pnpm-action` wants to challenge something in `action-starter`. Before filing that, check whether `nodejs-starter` (the parent's parent) already made a deliberate call here, or whether `react-starter` (the parent's other child) hit the same question and reached a different answer for a reason that would apply just as much here. If the challenge still holds up after that, it's worth raising; if `nodejs-starter` already tried it and reverted it for a good reason, the challenge doesn't hold and the child should just follow the existing norm instead.

A child cannot update its parent directly. To raise a validated challenge, file a GitHub issue on the immediate parent repo — background and rationale first, per `github-issue-pr-conventions` — laying out what's wrong with the current norm and what should replace it. Whether and how it actually gets adopted (in the parent, and from there elsewhere in the graph) is up to whoever handles that issue — not something to carry out directly from the child.

## Accessing Related Repos

There's no fixed convention for how related repos are reachable (local clone vs. `gh`/GitHub API) — ask the user if it's not already established for the current task.
