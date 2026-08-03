This file provides guidance to Claude Code (claude.ai/code) when working with code in any repository.

## Git Commits

Always sign commits with both GPG signing and the Developer Certificate of Origin sign-off, using the `gpg-no-prompt` wrapper so signing fails fast instead of hanging on a passphrase prompt:

```
git -c gpg.program=$HOME/.claude/gpg-no-prompt commit -sS -m "message"
```

- `-S` — GPG-signs the commit
- `-s` — appends a `Signed-off-by` trailer
- `gpg.program=$HOME/.claude/gpg-no-prompt` — signs in batch/loopback mode instead of opening an interactive pinentry prompt

If the commit fails with an error like `error: gpg failed to sign the data`, the GPG key is locked. Ask the user to unlock it by running `echo test | gpg --sign >/dev/null` themselves, then pause and wait for them to confirm before retrying the commit. Do not retry in a loop.

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

## Implementation Changes

When a fix requires changing the underlying implementation (e.g. replacing one approach with a different one, adding new abstractions, removing existing features), always describe the proposed change and the trade-offs first, then wait for explicit approval before making any edits. Do not conflate "explaining the root cause" with permission to implement a different solution.
