---
name: github-issue-pr-conventions
description: Formatting conventions for GitHub issue and pull request content — narration style, title/description casing, cross-repo issue/PR references. TRIGGER - about to run `gh issue create`, `gh pr create`, or otherwise write an issue body, PR title/description, or a comment referencing another repo's issue/PR. SKIP - commit messages (see git-commit-conventions) or repo descriptions (see repo-description-conventions).
---

## Cross-Repository References

When writing a GitHub issue, PR description, or comment that references an issue or PR in a different repository from the one the text is being written in, never use a bare `#id` — GitHub only auto-links a bare `#id` to the repository the text is rendered in, so it silently resolves to whatever issue or PR happens to hold that number there, which is very likely the wrong thing. Use either the full URL (`https://github.com/org/repo/issues/id`) or the `org/repo#id` shorthand instead — both render as an unambiguous cross-repo link.

A bare `#id` is only correct when referencing an issue or PR in the same repository as the text being written.

## GitHub Issues

When creating a GitHub issue, write it as narration that leads with the background and rationale — what problem or gap prompted the issue, what's currently missing, confusing, or broken, and why that matters. The implementation should get barely any space: a sentence or two near the end, or even folded into the closing line, is enough.

Do not impose a formal structure with headers like "Background" / "Rationale" / "Implementation" — write flowing prose instead, the way you'd explain the problem to a colleague.

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
