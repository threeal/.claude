---
name: repo-description-conventions
description: Formatting conventions for a GitHub repository's short description. TRIGGER - about to run `gh repo create --description`, `gh repo edit --description`, or otherwise write/update a repo's "About" description.
---

When writing or updating a repository's description (the short text shown under "About" on the repo page):

- Capitalize the first letter.
- Do not end with a period or any other trailing punctuation.
- Write it as a single tagline — a sentence fragment, not a full sentence with a verb clause and a period.
- Commas are fine for lightly joining related qualities (e.g. `Fast, minimal HTTP router for Go`).
- A single `-` or `—` separator is fine to join a name and a clarifying clause (e.g. `Zod — TypeScript-first schema validation`), but don't stack more than one.
- No emoji.
