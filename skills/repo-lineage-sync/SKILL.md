---
name: repo-lineage-sync
description: Process for catching a threeal repo up with changes its lineage parent made since the last sync — where to start, how to pick and port each change, and when the sync is done. TRIGGER - asked to sync, catch up, or bring a repo in line with its parent, or to port the parent's recent changes into a child repo.
---

Which repo is the parent, and what must stay uniform with it, come from `repo-lineage-uniformity`. Issues and PRs opened along the way follow `github-writing-conventions`.

## Starting

Compare the child with the parent's current `main`, and read the parent's issues and PRs since the last sync. Include drift from before the parent's latest release too, not just changes after it.

Lead with which change to port first and why it can go first, not a full inventory of every gap.
Why: the order is decided one step at a time against the child's real state, so an up-front inventory goes stale after the first merge.

## Each Step

Port one change at a time, and move to the next only once it's merged, until the child matches the parent.

Pick the next change by what it actually depends on in the child:

- Treat the parent's order as a starting guess, not proof. Check prerequisites against the child's real state — e.g. `pnpm why` for what pulls a package in — rather than reasoning about what probably does.
- Put a change that another change makes necessary into the same PR, such as cleanup left unused by an upgrade.
- Skip the parent's intermediate steps when only the end state matters, such as going straight from tsup to Rolldown without the parent's tsdown step.

Before porting, judge whether the parent's pattern actually fits the child. If it doesn't, say why and suggest what to do instead.

Open an issue only when the parent had one for the change; otherwise go straight to a PR.

- One child issue can cover several related parent issues, or one parent issue can be split across several.
- When the parent's issue follows a change from further up the lineage, link the issue where the change started, not the parent's: copy the parent's issue, including that link, and change only repo-specific details, like the current version in a quoted field.
  Why: the parent's issue usually adds little beyond a pointer to the ancestor's, so linking it only adds another click before the actual reasoning.
- Where the parent's reasoning doesn't hold in the child — such as an argument that only makes sense for a template — write the child's own reasoning instead.

Draft each issue in chat for review, and open it only once approved. Then implement on a new branch made from a freshly pulled `main`, and stop there for review.

## Finishing

Pull both repos and diff everything again. Report the sync as done only when every remaining difference is either deliberate to the child (its own application code, CI tests, README, and similar) or a dependency version.
