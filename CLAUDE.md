This file provides guidance to Claude Code (claude.ai/code) when working with code in any repository.

## Git Commits

Always sign commits with both GPG signing and the Developer Certificate of Origin sign-off:

```
git commit -S -s -m "message"
```

- `-S` — GPG-signs the commit
- `-s` — appends a `Signed-off-by` trailer

## Git Workflow

Never run `git commit` or `git push` unless explicitly told to do so. All changes must be reviewed before being committed.

Never run `git add` when asked to commit — the user may have already staged specific files, and unstaged changes during a commit are intentional.

## Pull Requests

When creating a pull request, capitalize the first letter of the PR title.

- Correct: `Add login feature`
- Incorrect: `add login feature`

Always write a meaningful PR description. Never open a PR with an empty or near-empty body — the point of asking Claude to open a PR is to avoid the user having to write the description themselves.

In the PR body, omit any test plan items already covered by CI (e.g. formatting, lint, type checking, tests). If all items would be covered by CI, skip the test plan section entirely.

## Commit Messages

When creating a commit, do not capitalize the first letter of the commit message.

- Correct: `add login feature`
- Incorrect: `Add login feature`
