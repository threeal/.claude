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

When it fails with `error: gpg failed to sign the data`, the key is locked. Ask the user to unlock it themselves with the command for their shell, then wait for their confirmation before retrying. Don't retry in a loop.

In a POSIX shell:

```sh
echo test | gpg --sign >/dev/null
```

In PowerShell on Windows, the gpg git signs with is Git for Windows' own, which isn't on PowerShell's PATH, and its default pinentry is a GUI dialog that a remote shell never shows. So call it by its full path and have it ask in the terminal, reading the data from a file to keep stdin free for the passphrase:

```powershell
"test" | Set-Content $env:TEMP\gpg-unlock.txt
& "C:\Program Files\Git\usr\bin\gpg.exe" --pinentry-mode loopback --yes --output $env:TEMP\gpg-unlock.sig --sign $env:TEMP\gpg-unlock.txt
```
