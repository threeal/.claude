---
name: git-branch-conventions
description: Naming for git branches. TRIGGER - about to create a branch with `git checkout -b`, `git switch -c`, or `git branch`, or otherwise pick a branch name.
---

## Naming

Name a branch after the specific change it carries, like a short version of its PR title, not after the topic area it touches.

- Correct: `default-version-to-tag`, for a PR titled "Default the version to a tag such as `latest` when it's not given"
- Incorrect: `version-tags`, for the same PR

Why: the name outlives the branch, since a merged PR keeps showing `merged … from <branch>` and offers to restore it. A topic name fits later work too, and once reused, `gh pr list --head <branch>` returns unrelated PRs and restoring the old branch collides with the new one.

Don't check whether the name is already taken; a name scoped to one change is unlikely to be reused for another.
