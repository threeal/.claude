---
name: git-conventions
description: Formatting and signing conventions for GitHub issues, git commits, GitHub pull requests, and GitHub repository descriptions. TRIGGER - about to run `gh issue create`, `git commit`, `gh pr create`, `gh repo create --description`, `gh repo edit --description`, or otherwise write an issue body, commit message, PR title/description, or repo description. SKIP - for the guardrails on whether to commit/push/stage at all; those live in CLAUDE.md and always apply.
---

## Cross-Repository References

When writing a GitHub issue, PR description, or comment that references an issue or PR in a different repository from the one the text is being written in, never use a bare `#id` — GitHub only auto-links a bare `#id` to the repository the text is rendered in, so it silently resolves to whatever issue or PR happens to hold that number there, which is very likely the wrong thing. Use either the full URL (`https://github.com/org/repo/issues/id`) or the `org/repo#id` shorthand instead — both render as an unambiguous cross-repo link.

A bare `#id` is only correct when referencing an issue or PR in the same repository as the text being written.

## GitHub Issues

When creating a GitHub issue, write it as narration that leads with the background and rationale — what problem or gap prompted the issue, what's currently missing, confusing, or broken, and why that matters. The implementation should get barely any space: a sentence or two near the end, or even folded into the closing line, is enough.

Do not impose a formal structure with headers like "Background" / "Rationale" / "Implementation" — write flowing prose instead, the way you'd explain the problem to a colleague.

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

Don't default to wrapping the whole body in a single `## Summary` header. A lone section header adds nothing when the content is obviously a summary — write it as plain prose or a bullet list with no header instead. Only add section headers when the body actually has more than one distinct kind of content to separate (e.g. a summary plus a test plan).

Omit the test plan section entirely unless there's something a human actually needs to do to verify the change by hand. In particular, skip it when:

- Every relevant check is already covered by CI or a git hook (formatting, lint, type checking, tests).
- The change is documentation-only or otherwise has no runtime behavior to exercise.
- It's otherwise obvious from the nature of the change that there's nothing to test.

Never include a test plan section just to say "N/A" or "nothing to test" — if there's nothing worth telling the reviewer to do, leave the section out rather than including it empty-handed.

## Repository Description

When writing or updating a repository's description (the short text shown under "About" on the repo page):

- Capitalize the first letter.
- Do not end with a period or any other trailing punctuation.
- Write it as a single tagline — a sentence fragment, not a full sentence with a verb clause and a period.
- Commas are fine for lightly joining related qualities (e.g. `Fast, minimal HTTP router for Go`).
- A single `-` or `—` separator is fine to join a name and a clarifying clause (e.g. `Zod — TypeScript-first schema validation`), but don't stack more than one.
- No emoji.
