---
name: git-branch-conventions
description: Choosing the base and the name for git branches. TRIGGER - about to start implementing a change meant for its own PR, create a branch with `git checkout -b`, `git switch -c`, or `git branch`, or otherwise pick a branch's base or name.
---

## Base

Start new work from the latest default branch, not from whatever branch is checked out. That's often the previous PR's branch, left behind while that PR is still unmerged.

When the work builds on an unmerged PR, do one of two things:

- implement it against the default branch alone, without that PR's change;
- name the PR it needs, and let the user decide whether to merge that PR first or stack the new work on it.

Stack a branch or PR on another only when the user explicitly says to.
Why: stacking only settles merge order, and pays off only when something has to be built quickly on a dependency that can't merge yet. That's the user's call, not something implied by the dependency being checked out or by the new work using its code.

## Naming

Name a branch after the specific change it carries, like a short version of its PR title, not after the topic area it touches.

- Correct: `default-version-to-tag`, for a PR titled "Default the version to a tag such as `latest` when it's not given"
- Incorrect: `version-tags`, for the same PR
- Correct: `bump-setup-pnpm-action-v2.1.0`, for a PR bumping `setup-pnpm-action` to v2.1.0
- Incorrect: `bump-setup-pnpm-action`, for the same PR, since it fits every later bump too

Why: the name outlives the branch, since a merged PR keeps showing `merged … from <branch>` and offers to restore it. A topic name fits later work too, and once reused, `gh pr list --head <branch>` returns unrelated PRs and restoring the old branch collides with the new one.

Don't check whether the name is already taken; a name scoped to one change is unlikely to be reused for another.
