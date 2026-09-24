---
name: git-commit-conventions
description: Signing, message content, and casing for git commits. TRIGGER - about to run `git commit` or write a commit message.
---

## Signing

Sign every commit with both GPG and the DCO sign-off, through the `gpg-no-prompt` wrapper:

```
git -c gpg.program=$HOME/.claude/gpg-no-prompt commit -sS -m "message"
```

Why: the wrapper signs in batch/loopback mode, so a locked key fails fast instead of hanging on an interactive pinentry prompt.

When it fails with `error: gpg failed to sign the data`, the key is locked. Ask the user to unlock it by running `echo test | gpg --sign >/dev/null` themselves, then wait for their confirmation before retrying. Don't retry in a loop.

## Commit Messages

Default to a subject line with no body. A body is rare — write one only when the subject can't carry the change on its own.

Never restate what the issue or PR already covers, such as background, rationale, or a full account of the work.
Why: the commit sits one click from both, so repeating any of it only duplicates what a reader can already reach.

Never reference an issue or PR in the body, in any form — `Closes #123`, `fixes #123`, or a bare `#123`.
Why: the `(#NN)` suffix on the squashed subject already chains the commit to its PR and the PR to its issue, and a reference in the body adds a second cross-reference to that issue's timeline for the same work.

Trailers are exempt from all of the above; the signing rule above requires them.

Don't capitalize the first letter.

- Correct: `add login feature`
- Incorrect: `Add login feature`
