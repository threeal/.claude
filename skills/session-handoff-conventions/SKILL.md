---
name: session-handoff-conventions
description: Where and how to save a session's unfinished work so another session, machine, or Claude account can continue it, and how to resume and clean up that save. TRIGGER - the user says they're leaving, closing, or stepping away from the session, asks to save, checkpoint, resume, continue, or hand off a session's context or work, or points at a session gist.
---

## Where a Save Goes

Save a session's context to a secret gist on the user's GitHub account, not an artifact or a local file.
Why: the GitHub account is what every machine and Claude account share, and the work is usually tied to a GitHub repo anyway.

Keep one gist per repo, its description set to the repo's full name (e.g. `threeal/.claude`), so `gh gist list` shows which repos have unfinished work.
For work not tied to a repo, use the name of the directory the session runs in instead, not its full path.

Each piece of work is its own Markdown file in that gist, named after the work (e.g. `macos-install-entries.md`) and opening with a `#` title saying what the work is about.

A secret gist is unlisted, not private: anyone with its URL can read it, so never put tokens, credentials, or anything else sensitive in a save.

## Saving

Save only when the user is about to leave the session or asks for a save, never on your own initiative.

A save is a checkpoint, not a live log. After resuming from a gist, never update it or suggest updating it while the work goes on; the next save, when the user leaves again, replaces that work's file.

A save holds what a fresh session with no memory of this one needs to continue:

- The goal, and the decisions made so far with why.
- What's done, with issue and PR links.
- What's next, and open questions.
- The branch, any uncommitted changes, and the machine they're on, since local work doesn't travel with the save.

Write the file in the scratchpad under its gist file name, then upload it:

- New gist, which `gh` makes secret by default: `gh gist create --desc '<repo>' <file>`
- New file in an existing gist: `gh gist edit <id> --add <file>`
- Replacing a saved file: `gh gist edit <id> --filename <name> <file>`

## Resuming

Find the gist by its description with `gh gist list`, read the work's file with `gh gist view <id> --filename <name> --raw`, and check that the branch and uncommitted changes it names are still there before carrying on.

## Cleaning Up

Once the work is done, suggest removing its file with `gh gist edit <id> --remove <name>`, or, when it's the gist's last file, deleting the whole gist with `gh gist delete <id> --yes`.
Delete only after the user confirms.
