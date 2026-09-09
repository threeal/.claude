---
name: writing-project-claude-md
description: What to include and exclude when writing or updating a CLAUDE.md for a project other than ~/.claude. TRIGGER - about to create or edit a CLAUDE.md file in any repository other than ~/.claude itself. SKIP - editing ~/.claude/CLAUDE.md or ~/.claude/skills/ (see claude-home-conventions for that).
---

When writing or updating a CLAUDE.md for a project, balance documenting details against letting files be read directly:

- Don't restate details that are readily read from a file (e.g. exact contents, field lists, code) — restating grows CLAUDE.md and rots whenever the file changes. Re-reading a file costs time/context, but that's cheaper than a stale or bloated CLAUDE.md.
- Do capture how files/directories relate to each other and each one's purpose, so the project's shape is clear without reading everything.
- Not every task needs every file's fine detail. Document at the "map" level — what exists, why, how pieces relate — and delegate fine detail to reading the file when a task actually needs it.
- Before writing, mentally set aside anything known only from this session, from `~/.claude`, or from any other personal/global source — write as if starting fresh, with no memory of who's asking or what machine/session this is. A project's CLAUDE.md must read the same and hold up for any new session, on any machine, for any user working in that repo — not just the current one.
