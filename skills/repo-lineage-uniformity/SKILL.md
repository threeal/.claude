---
name: repo-lineage-uniformity
description: Keep shared config, docs, and structure uniform across the hierarchy of related threeal repos derived from one another. TRIGGER - about to edit a config file, README, workflow, or other shared-style file in a repo whose GitHub remote is under github.com/threeal. SKIP - editing repo-specific application code/content with no shared counterpart elsewhere in the graph, or a repo confirmed not part of this ecosystem.
---

Repos under https://github.com/threeal aren't standalone — each can derive from a parent and have children of its own, whether or not it's a template. Every edge is the same kind of relationship under the same rule; don't treat template-to-template and template-to-non-template edges differently.

## The Known Graph

```
project-starter -> nodejs-starter
nodejs-starter -> action-starter
nodejs-starter -> react-starter
action-starter -> setup-lefthook-action
action-starter -> setup-pnpm-action
react-starter -> greenwall
```

This list is deliberately incomplete, and extending it is a separate task from this skill's job — don't deduce a repo's position by exploring the user's account. In a repo under `github.com/threeal` that isn't listed, ask whether it belongs and where, or whether it simply isn't part of this system; if it isn't, none of the rest applies.

## What Must Stay Uniform

Shared elements — config files, documentation wording/style/tone, structural ordering, and similar generic conventions — should be implemented identically between a repo and its parent, unless a specific part was deliberately chosen to differ. This doesn't extend to genuinely repo-specific content with no counterpart elsewhere, such as `react-starter`'s React setup having no equivalent in `action-starter`.

## Default: Follow the Parent

Adopt whatever convention the immediate parent already uses, comparing against that parent alone.
Why: routine work shouldn't cost a scan of siblings, cousins, and the rest of the graph.
This is a default, not an obligation — a child can challenge its parent instead, once the challenge is validated.

## Challenging a Parent

Before raising a challenge, validate it by scanning outward — the parent's own parent, other ancestors, and siblings, as far as is relevant — and raise it only if it survives that check.
Why: this is a fact-check, not a propagation step. A grandparent may have tried the same idea and reverted it for a reason that still applies here, or a sibling may have settled the same question differently for a reason worth knowing.

A child cannot update its parent directly. File a GitHub issue on the immediate parent repo — background and rationale first, per `github-writing-conventions` — laying out what's wrong with the current norm and what should replace it. Whether it gets adopted, in the parent and onward through the graph, is up to whoever handles that issue.

## Accessing Related Repos

There's no fixed convention for how related repos are reachable, local clone versus `gh`/GitHub API — ask the user when it isn't already established for the task.
