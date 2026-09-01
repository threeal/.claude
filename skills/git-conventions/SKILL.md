---
name: git-conventions
description: Formatting and signing conventions for git commits and GitHub pull requests. TRIGGER - about to run `git commit`, `gh pr create`, or otherwise write a commit message or PR title/description. SKIP - for the guardrails on whether to commit/push/stage at all; those live in CLAUDE.md and always apply.
---

## Git Commits

Always sign commits with both GPG signing and the Developer Certificate of Origin sign-off, using the `gpg-no-prompt` wrapper so signing fails fast instead of hanging on a passphrase prompt:

```
git -c gpg.program=$HOME/.claude/gpg-no-prompt commit -sS -m "message"
```

- `-S` — GPG-signs the commit
- `-s` — appends a `Signed-off-by` trailer
- `gpg.program=$HOME/.claude/gpg-no-prompt` — signs in batch/loopback mode instead of opening an interactive pinentry prompt

If the commit fails with an error like `error: gpg failed to sign the data`, the GPG key is locked. Ask the user to unlock it by running `echo test | gpg --sign >/dev/null` themselves, then pause and wait for them to confirm before retrying the commit. Do not retry in a loop.

## Commit Messages

When creating a commit, do not capitalize the first letter of the commit message.

- Correct: `add login feature`
- Incorrect: `Add login feature`

## Pull Requests

When creating a pull request, capitalize the first letter of the PR title.

- Correct: `Add login feature`
- Incorrect: `add login feature`

Always write a meaningful PR description. Never open a PR with an empty or near-empty body — the point of asking Claude to open a PR is to avoid the user having to write the description themselves.

In the PR body, omit any test plan items already covered by CI (e.g. formatting, lint, type checking, tests). If all items would be covered by CI, skip the test plan section entirely.
