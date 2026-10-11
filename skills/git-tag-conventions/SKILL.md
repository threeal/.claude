---
name: git-tag-conventions
description: Signing for git tags. TRIGGER - about to create a tag with `git tag`.
---

## Signing

Sign every tag with GPG, through the `gpg-no-prompt` wrapper:

```
git -c gpg.program=$HOME/.claude/gpg-no-prompt tag -s v1.2.3 -m "message"
```

Why: the wrapper signs in batch/loopback mode, so a locked key fails fast instead of hanging on an interactive pinentry prompt.

When it fails with `error: gpg failed to sign the data`, the key is locked. Ask the user to unlock it by running `echo test | gpg --sign >/dev/null` themselves, then wait for their confirmation before retrying. Don't retry in a loop.
