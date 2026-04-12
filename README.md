# Mise Development Setup

This repo is the portable userland layer for my development environment.

It keeps the OS-specific setup intentionally thin and puts the reusable tooling in `mise`, while `Neovim` keeps using Mason for editor-only LSPs, formatters, and related tooling.

## Update submodules

Upon cloning pull latest submodules with:

```bash
git submodule update --init --remote --recursive
```

## Prerequisites

Get a decent terminal emulator before doing anything else.

- Linux and macOS: use `ghostty`
- Windows: use `wezterm`

The default Windows path in this repo is still `wezterm` plus WSL, but native Windows also works for the shared `mise` and `Neovim` setup below.

### Linux and WSL

Install these with your distro package manager:

- `git`
- `curl` or `wget`
- `unzip`
- `tar`
- `gzip`
- `make`

Clipboard integration is assumed to be handled by the host OS.

### Windows

Install these first:

- `git` for Windows
- `Git Bash`
- `wezterm`

Using `Git Bash` keeps the shell setup close to the Linux and WSL instructions. You can run it directly, or launch it inside `wezterm`.

Example `wezterm` config:

```lua
config.default_prog = { "C:/Program Files/Git/bin/bash.exe", "-l" }
```

### macOS

Install Xcode Command Line Tools first:

```bash
xcode-select --install
```

That covers the usual `git`, `tar`, and `make` requirements.

## Install Mise

Install `mise`:

### Linux and macOS

```bash
curl https://mise.run | sh
```

### Windows

Use either:

```powershell
scoop install mise
```

or:

```powershell
winget install jdx.mise
```

Activate it in your shell.

### Bash

```bash
echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc
```

### Zsh

```bash
echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc
```

### Fish

```fish
echo '~/.local/bin/mise activate fish | source' >> ~/.config/fish/config.fish
```

### Git Bash on Windows

```bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc
```

Open a new shell afterwards.

## Install The Toolchain

Copy the config into place:

```bash
mkdir -p ~/.config/mise
cp mise/config.toml ~/.config/mise/config.toml
```

On native Windows from `Git Bash`, `~/.config/mise` is fine too. Run `mise config` if you want to confirm the exact resolved path on your machine.

Then install everything:

```bash
mise install
```

This config manages:

- `neovim`
- `node`
- `ripgrep`
- `tree-sitter`
- `rust`
- `bun`
- `cmake`
- `clang`
- `go`
- `github-cli`
- `ninja`
- `zig`
- `uv`
- `typst`
- `odin`
- `swift`

## Neovim Setup

Copy the `Neovim` config:

```bash
mkdir -p ~/.config
cp -r nvim ~/.config/
```

Launch it to install:

```bash
nvim
```

On native Windows, `Neovim` itself still uses its normal Windows config location under `%LOCALAPPDATA%\nvim`, so treat the `~/.config` copy step above as the `Git Bash` equivalent path rather than a guarantee about where `stdpath("config")` resolves.

> Note: the current neovim config only looks for python interpreters in unix locations, so the `Scripts/python.exe` location in windows may not work without a future fix

## Docker Notes

### Linux and WSL

For Docker, use your distro package manager. That usually installs the matching CLI together with the engine.

### macOS

For Docker without Docker Desktop:

```bash
brew install colima docker
colima start
```
