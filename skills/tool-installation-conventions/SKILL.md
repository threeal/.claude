---
name: tool-installation-conventions
description: Approved install commands for CLI tools per platform, and how to install one mid-task without blocking it. TRIGGER - a command fails with "command not found", or a task needs a CLI tool that isn't installed (e.g. cargo, rustc, rustup, uv, poetry, pnpm, node, dprint).
---

## Installing a Listed Tool

When a needed tool is missing and Tools below lists it for the current platform, install it right away without asking, running exactly the listed commands.
Why: the listed commands fix where each tool comes from, so every machine gets it from the same source.

Tell the platform from `/etc/os-release` and `uname -m`.
If the tool, the platform, or a prerequisite (e.g. an apt package needing `sudo`) isn't covered here, stop: name what's missing, and give the command for the user to run when it's known.

## After Installing

Check the tool in a fresh interactive shell, which reads the shell config the way a new terminal does:

```sh
fish -ic 'command -v <tool>'
```

If fish can't find it, try `bash -ic 'command -v <tool>'`. Fish is launched from bash, so a PATH set in the bash config also reaches fish.

If neither finds it, fix the shell setup the installer left — its output usually says where it added PATH or what to add — then check again, and suggest adding the fix to that tool's entry below.

Keep working without waiting on a restart. The current session's commands still run on the environment from before the install, so make the tool available within each command that needs it, without touching anything global — e.g. `. "$HOME/.cargo/env" && cargo build`. Once the task is done, suggest the user restart the shell and Claude Code.

That workaround belongs only to the session that did the install. In a later session, a tool that's installed but not found means its shell setup is broken — fix that instead.

## Tools

### Rust (cargo, rustc, rustup)

Ubuntu x64:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
```

### uv

Ubuntu x64:

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Poetry

Requires pipx.

Ubuntu x64:

```sh
pipx install poetry
```

### pnpm

Ubuntu x64:

```sh
curl -fsSL https://get.pnpm.io | sh -
```

### Node

Requires pnpm; install it first if it's missing too.

Ubuntu x64:

```sh
pnpm runtime set node lts -g
```

### dprint

Ubuntu x64:

```sh
curl -fsSL https://dprint.dev/install.sh | sh
```

The installer doesn't touch any shell config, so put dprint on the PATH in `~/.config/fish/conf.d/dprint.fish`:

```fish
set -gx DPRINT_INSTALL "$HOME/.dprint"
fish_add_path -g "$DPRINT_INSTALL/bin"
```
