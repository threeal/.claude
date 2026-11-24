---
name: tool-installation-conventions
description: Approved install commands for CLI tools per platform, and how to install one mid-task without blocking it. TRIGGER - a command fails with "command not found", or a task needs a CLI tool that isn't installed (e.g. brew, cargo, cargo-tarpaulin, dprint, gh, go, golangci-lint, gradle, java, lefthook, node, pnpm, poetry, rustc, rustup, sdk, uv, winget).
---

## Installing a Listed Tool

When a needed tool is missing and Tools below lists it for the current platform, install it right away without asking, running exactly the listed commands.
Why: the listed commands fix where each tool comes from, so every machine gets it from the same source.

Tell the platform from `/etc/os-release` and `uname -m`.
On macOS, where `/etc/os-release` doesn't exist, `uname -s` prints `Darwin` instead, and `uname -m` prints `arm64` on Apple silicon.
On Windows, where neither exists, tell it from `$env:OS` (`Windows_NT`) and `$env:PROCESSOR_ARCHITECTURE` (`AMD64` for x64) in PowerShell.
If the tool, the platform, or a prerequisite (e.g. an apt package needing `sudo`) isn't covered here, stop: name what's missing, and give the command for the user to run when it's known.

A step that needs `sudo` or other interactive input is the user's to run: give it as one command per step, to run in a separate terminal.
Why: `! <command>` has no terminal to prompt in, so `sudo` fails there with "a terminal is required to read the password".
Over Windows OpenSSH, an administrator account's session is already elevated, so a step needing admin rights isn't the user's to run there.

## After Installing

Check the tool in a fresh interactive session of the user's default shell, which reads the shell config the way a new terminal does:

```sh
"$SHELL" -ic 'command -v <tool>'
```

`$SHELL` is the login shell: usually bash on Linux and zsh on macOS. When fish runs on top of it, fish inherits the PATH it sets, so a tool found there reaches fish too. When fish is the default shell itself, `$SHELL` is fish.

If it isn't found, fix the shell setup the installer left — its output usually says where it added PATH or what to add — then check again. Suggest adding the fix to that tool's entry below only when the installer's output doesn't make it clear, such as an installer that touches no shell config and says nothing about it.
An entry's shell setup goes in the default shell's own config: `~/.bashrc` for bash, `~/.zshrc` for zsh, or `~/.config/fish/conf.d/<tool>.fish` for fish.

Keep working without waiting on a restart. The current session's commands still run on the environment from before the install, so make the tool available within each command that needs it, without touching anything global — e.g. `. "$HOME/.cargo/env" && cargo build`. Once the task is done, suggest the user restart the shell and Claude Code.

That workaround belongs only to the session that did the install. In a later session, a tool that's installed but not found means its shell setup is broken — fix that instead.

On Windows, the default shell is PowerShell, and the same steps apply, checking with `Get-Command <tool>`, except that the check can't run in a shell started from the current one. A shell started before the install doesn't see the PATH change the installer made, and a child shell inherits that stale PATH, so call a newly installed tool by its full path until the shell is restarted, then check it.

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

The installer doesn't touch any shell config, so in the default shell's config, set `DPRINT_INSTALL` to `$HOME/.dprint` and put `$DPRINT_INSTALL/bin` on the PATH.

### gh

macOS arm64, which requires Homebrew; install it first if it's missing too.
Install it, which puts it in `/opt/homebrew/bin`, already on the PATH from Homebrew's own setup:

```sh
brew install gh
```

Then give the user `gh auth login` to run.

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

Windows x64, which requires winget; install it first if it's missing too.
Install it, which puts `gh.exe` in `C:\Program Files\GitHub CLI\`:

```powershell
winget install --id GitHub.cli --source winget
```

Then give the user `gh auth login` to run.
Over SSH, no browser opens on the remote machine, so they log in through the one-time code and URL it prints instead.

Besides finding `gh`, check the login with `gh auth status`.

### Go

Ubuntu x64, from the official tarball, with `<version>` the latest release from `https://go.dev/VERSION?m=text` (e.g. `go1.25.4`).
Download it:

```sh
curl -fsSLO https://go.dev/dl/<version>.linux-amd64.tar.gz
```

Check the tarball's SHA-256 against the one `https://go.dev/dl/?mode=json` lists for it, and extract it only if they match:

```sh
tar -C ~/.local -xzf <version>.linux-amd64.tar.gz
```

The tarball doesn't touch any shell config, so in the default shell's config, put Go and the binaries from `go install` on the PATH: `$HOME/.local/go/bin` and `$HOME/go/bin`.

### golangci-lint

Requires Go; install it first if it's missing too.

Ubuntu x64, from the official install script, into the bin directory `go install` uses, which the Go entry already puts on the PATH:

```sh
curl -sSfL https://golangci-lint.run/install.sh | sh -s -- -b ~/go/bin
```

The script checks the release tarball's SHA-256 itself.
Don't install it with `go install`; golangci-lint's docs advise against building it from source.

### Gradle

Requires SDKMAN!; install it first if it's missing too.

Ubuntu x64:

```sh
source ~/.sdkman/bin/sdkman-init.sh && sdk install gradle
```

### Homebrew (brew)

macOS arm64, from the official install script.
It asks for the user's password to run `sudo`, so give the user this to run:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Then have them run the "Next steps" commands it prints, which put `/opt/homebrew/bin` on the PATH.

### Java (JDK)

Requires SDKMAN!; install it first if it's missing too.

Ubuntu x64, picking the Temurin version from the identifiers `sdk list java` shows:

```sh
source ~/.sdkman/bin/sdkman-init.sh && sdk install java <version>-tem
```

### Lefthook

macOS arm64, which requires Homebrew; install it first if it's missing too.
Install it, which puts it in `/opt/homebrew/bin`, already on the PATH from Homebrew's own setup:

```sh
brew install lefthook
```

### Node

Requires pnpm; install it first if it's missing too.

Ubuntu x64:

```sh
pnpm runtime set node lts -g
```

### pnpm

macOS arm64 and Ubuntu x64:

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

macOS arm64 and Ubuntu x64:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
```

### SDKMAN! (sdk)

Ubuntu x64:

```sh
curl -s "https://get.sdkman.io" | bash
```

SDKMAN! only hooks into bash and zsh, so fish finds its tools only when it runs on top of one of them and inherits its PATH.
`sdk` itself is a bash function rather than a binary, so in a non-interactive shell, run `source ~/.sdkman/bin/sdkman-init.sh` before any `sdk` command, and check it with `bash -ic 'type sdk'` instead of `command -v`.

### uv

macOS arm64 and Ubuntu x64:

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### winget

Windows x64, in PowerShell, without admin rights.
Register App Installer, which puts winget at `%LOCALAPPDATA%\Microsoft\WindowsApps\winget.exe`, then install the winget source's data package:

```powershell
Add-AppxPackage -RegisterByFamilyName -MainPackage Microsoft.DesktopAppInstaller_8wekyb3d8bbwe
Add-AppxPackage -Path https://cdn.winget.microsoft.com/cache/source.msix
```

Without the second, every install fails with `0x8a15000f : Data required by the source is missing`, which neither `winget source update` nor `winget source reset --force` fixes.
