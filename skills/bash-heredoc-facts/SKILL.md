---
name: bash-heredoc-facts
description: Facts about bash heredoc quoting — what a quoted delimiter disables vs. what an unquoted one still expands. TRIGGER - about to write a heredoc to embed multi-line text (a commit message, PR/issue body, `gh api` payload, or script) that may contain backticks, `$`, `"`, or `\`.
---

## Quoted Delimiters Disable All Expansion

A heredoc whose delimiter is quoted (`<<'EOF'`) is a literal block: no variable substitution, no command substitution, and no backslash-escape processing. A character that would need escaping inside a double-quoted string — backtick, `$`, `"`, `\` — needs none here; a backslash placed before one survives into the output literally instead of being consumed.

An unquoted delimiter (`<<EOF`) still expands `$var`, `` `cmd` ``, and `$(cmd)` inside the block, so only those need escaping there.

Why: the reflex to escape special characters carries over from double-quoted strings without checking whether the surrounding quoting makes it necessary. In a quoted heredoc that reflex is actively wrong — `` \` `` publishes as a literal backslash followed by a backtick instead of rendering as a backtick, since nothing consumes the backslash.

- Incorrect, inside `<<'EOF'`: ``See \`README.md\` for details.`` — renders with the backslashes still in it.
- Correct, inside `<<'EOF'`: ``See `README.md` for details.`` — renders as intended.

Prefer the quoted form when embedding markdown or code verbatim — `git commit -m "$(cat <<'EOF' ... EOF)"`, `gh issue create --body "$(cat <<'EOF' ... EOF)"`, a `gh pr create --body`, or a `gh api` JSON payload — so the content needs no escaping consideration at all.
