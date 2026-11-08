---
name: tool-installation-conventions
description: Approved install commands for CLI tools per platform, and how to install one mid-task without blocking it. TRIGGER - a command fails with "command not found", or a task needs a CLI tool that isn't installed (e.g. cargo, cargo-tarpaulin, dprint, gh, gradle, java, node, pnpm, poetry, rustc, rustup, sdk, uv).
---

## Installing a Listed Tool

When a needed tool is missing and Tools below lists it for the current platform, install it right away without asking, running exactly the listed commands.
Why: the listed commands fix where each tool comes from, so every machine gets it from the same source.

Tell the platform from `/etc/os-release` and `uname -m`.
If the tool, the platform, or a prerequisite (e.g. an apt package needing `sudo`) isn't covered here, stop: name what's missing, and give the command for the user to run when it's known.

A step that needs `sudo` or other interactive input is the user's to run: give it as one command per step, to run in a separate terminal.
Why: `! <command>` has no terminal to prompt in, so `sudo` fails there with "a terminal is required to read the password".

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

### cargo-tarpaulin

Requires Rust; install it first if it's missing too.

Ubuntu x64, which also needs `pkg-config` and `libssl-dev` from apt to build its OpenSSL dependency.
Check them with `dpkg -s pkg-config libssl-dev` before starting the build, and if either is missing, give the user this to run first:

```sh
sudo apt install -y pkg-config libssl-dev
```

Then:

```sh
cargo install cargo-tarpaulin
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

### gh

Ubuntu x64, from GitHub's apt repository. Prepare its keyring and source file in the scratchpad yourself:

```sh
wget -nv -O githubcli-archive-keyring.gpg https://cli.github.com/packages/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" > github-cli.list
```

Then give the user the rest, one command at a time, with the scratchpad paths filled in:

```sh
sudo install -m 644 <scratchpad>/githubcli-archive-keyring.gpg /etc/apt/keyrings/
sudo install -m 644 <scratchpad>/github-cli.list /etc/apt/sources.list.d/
sudo apt update
sudo apt install gh
gh auth login
```

Besides finding `gh`, check the login with `gh auth status`.

### Gradle

Requires SDKMAN!; install it first if it's missing too.

Ubuntu x64:

```sh
source ~/.sdkman/bin/sdkman-init.sh && sdk install gradle
```

### Java (JDK)

Requires SDKMAN!; install it first if it's missing too.

Ubuntu x64, picking the Temurin version from the identifiers `sdk list java` shows:

```sh
source ~/.sdkman/bin/sdkman-init.sh && sdk install java <version>-tem
```

### Node

Requires pnpm; install it first if it's missing too.

Ubuntu x64:

```sh
pnpm runtime set node lts -g
```

### pnpm

Ubuntu x64:

```sh
curl -fsSL https://get.pnpm.io | sh -
```

### Poetry

Requires pipx.

Ubuntu x64:

```sh
pipx install poetry
```

### Rust (cargo, rustc, rustup)

Ubuntu x64:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
```

### SDKMAN! (sdk)

Ubuntu x64:

```sh
curl -s "https://get.sdkman.io" | bash
```

SDKMAN! only hooks into bash and zsh, so fish finds its tools only through the PATH it inherits from bash.
`sdk` itself is a bash function rather than a binary, so in a non-interactive shell, run `source ~/.sdkman/bin/sdkman-init.sh` before any `sdk` command, and check it with `bash -ic 'type sdk'` instead of `command -v`.

### uv

Ubuntu x64:

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```
