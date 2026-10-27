---
name: github-release-conventions
description: Conventions for GitHub releases — whether a change warrants one, the linked Announcements discussion, title, tag, and release notes format. TRIGGER - deciding whether a change warrants a release, about to run `gh release create` or `gh release edit`, or otherwise create or publish a GitHub release.
---

## Deciding Whether to Release

Release only when something the repo ships to its users changed:

- A template ships itself — its files, tooling and setup — to derived projects, so a tooling-only change such as a new lint rule or a reordered pre-commit hook warrants a release.
- Any other repo, such as a library, app or action, ships what it exports or outputs, so a tooling-only change gets no release of its own and waits for the next release that changes what's published.

Each repo decides for itself, so catching a repo up with a release of its parent in the `repo-lineage-uniformity` graph doesn't warrant a release of its own unless what it ships changed.
Why: a release with nothing new for its users only adds noise to a version history that's supposed to track what they get.

## Discussion

Create every release with `--discussion-category Announcements`, so the release creates its own Announcements discussion, titled after it and linked from it:

```
gh release create v1.2.3 --verify-tag --title "Version 1.2.3" --discussion-category Announcements --notes-file notes.md
```

When a release was already published without one, fix it with `gh release edit v1.2.3 --discussion-category Announcements`, never by opening a discussion manually.
Why: a manually opened discussion isn't linked to the release, so it doesn't show up on the release page, and its title and body have to be copied over instead of coming from the release.

## Title and Tag

Title a release `Version X.Y.Z`, for tag `vX.Y.Z`.

Create the tag as a signed annotated tag whose message is the same `Version X.Y.Z` as the release title — see `git-tag-conventions` for how to sign it.

## Release Notes

```
## What's Changed

### Breaking Changes

- **Label** — what users must change, and why ...

### New Features

- **Label** — the action now ...

### Improvements

- **Label** — ... is now ..., so ...

### Bug Fixes

- **Label** — ... no longer ...

### Dependency Updates

- `package` bumped to 1.2.3

### Development Changes

- **Label** — the bundler is now ...

**Full Changelog**: https://github.com/<owner>/<repo>/compare/v1.2.2...v1.2.3
```

Omit any `###` section with nothing in it.

Write every entry except dependency bumps as a short bold label naming the change, an em dash, then a present-tense description of how things behave now and, when it isn't obvious, what that means for the reader.

Put a fixed defect under `Bug Fixes`, never `Improvements`, so a patch release shows what it fixed and a minor release shows whether it's worth upgrading for.

List only shipped dependencies under `Dependency Updates`. Tooling changes that don't reach users — bundler, CI, lint rules, devDependency bumps — go under `Development Changes`, which never on its own warrants a release (see Deciding Whether to Release above).
