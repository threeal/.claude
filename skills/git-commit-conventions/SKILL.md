---
name: git-commit-conventions
description: Signing and message casing for git commits. TRIGGER - about to run `git commit` or write a commit message. SKIP - whether to commit/push/stage at all; those guardrails live in CLAUDE.md and always apply.
---

## Signing

Sign every commit with both GPG and the DCO sign-off, through the `gpg-no-prompt` wrapper:

```
git -c gpg.program=$HOME/.claude/gpg-no-prompt commit -sS -m "message"
```

Why: the wrapper signs in batch/loopback mode, so a locked key fails fast instead of hanging on an interactive pinentry prompt.

When it fails with `error: gpg failed to sign the data`, the key is locked. Ask the user to unlock it by running `echo test | gpg --sign >/dev/null` themselves, then wait for their confirmation before retrying. Don't retry in a loop.

## Commit Messages

Don't capitalize the first letter.

- Correct: `add login feature`
- Incorrect: `Add login feature`
