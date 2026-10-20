---
name: ordering-conventions
description: How to choose where an entry goes when its order doesn't affect behavior — list items, config keys and entries, action inputs, paths, and similar. TRIGGER - about to add, move, or reorder an item in a list, a markdown list, a config or manifest file, a workflow step's inputs, or any other sequence whose order the tools ignore.
---

## Choose the Position Deliberately

When order doesn't affect behavior, place each entry by a deliberate order, never wherever is convenient — appended at the end, next to the last edit, or in the order it came to mind.
Why: a convenient position looks arbitrary to a reader, and correcting it afterwards costs a review round each time.

Take the order from a reference whenever one exists:

- Config keys, action inputs, and similar entries: the tool's own reference or docs, such as the input order in an action's `action.yml`.
- Anything else: the lineage parent's counterpart (see `repo-lineage-uniformity`), or a convention the file's existing entries already follow.

Without a reference, default to alphabetical. For paths, that means directories before files, then alphabetical by each path segment — `tools/go.mod` before `go.mod`, and `cmd/bonacci/` before `CLAUDE.md`.

None of this is strict; a different order is fine when there's a reason for it. What matters is that the position is decided after looking for a reference first.
