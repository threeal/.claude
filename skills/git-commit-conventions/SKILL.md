---
name: git-commit-conventions
description: Signing and message-casing conventions for git commits. TRIGGER - about to run `git commit` or write a commit message. SKIP - whether to commit/push/stage at all; those guardrails live in CLAUDE.md and always apply.
---

## Signing

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
