---
name: github-issue-pr-conventions
description: Formatting conventions for GitHub issue and pull request content — narration style, title/description casing, cross-repo issue/PR references. TRIGGER - about to run `gh issue create`, `gh pr create`, or otherwise write an issue body, PR title/description, or a comment referencing another repo's issue/PR. SKIP - commit messages (see git-commit-conventions) or repo descriptions (see repo-description-conventions).
---

## Naming vs. Linking References

Backticks are for naming something as an identifier — a repo, an issue/PR, a file — and are fine there regardless of whether that same thing is linkable elsewhere. Whether a reference should also be clickable is a separate choice: match it to whether a click-through is actually wanted at that point in the text, not an absolute ban on backticks.

For an issue or PR, an actual link relies on GitHub's autolinking, which only fires on plain text — never inside a code span:

- Same-repo: a bare `#id` in plain text.
- Cross-repo: `org/repo#id` or the full URL, in plain text. Never a bare `#id` cross-repo — GitHub auto-links it to whatever issue or PR happens to hold that number in the rendering repo, which is very likely the wrong thing.

Naming an issue/PR rather than linking it — e.g. in a possessive phrase, or when it's already linked once elsewhere in the same body — is fine backticked either way: `` `threeal/action-starter#1076` ``'s CLAUDE.md implementation replaced...

For a repo or a file, GitHub doesn't auto-link a bare string at all, so naming one — backticked or not — never produces a link. If a repo or file actually needs to be clickable, write an explicit markdown link to its GitHub URL instead:

- Naming a repo: `` `threeal/nodejs-starter` ``'s CLAUDE.md still uses...
- Linking a repo: see [threeal/nodejs-starter](https://github.com/threeal/nodejs-starter)
- Naming a file: `` `src/main.ts` `` handles the entry point
- Linking a file: see [`src/main.ts`](https://github.com/org/repo/blob/main/src/main.ts)

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
