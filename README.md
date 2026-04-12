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
- Windows: use `wezterm`, then do the actual setup inside WSL

### Linux and WSL

Install these with your distro package manager:

- `git`
- `curl` or `wget`
- `unzip`
- `tar`
- `gzip`
- `make`

Clipboard integration is assumed to be handled by the host OS.

### macOS

Install Xcode Command Line Tools first:

```bash
xcode-select --install
```

That covers the usual `git`, `tar`, and `make` requirements.

## Install Mise

Install `mise`:

```bash
curl https://mise.run | sh
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

Open a new shell afterwards.

## Install The Toolchain

Copy the config into place:

```bash
mkdir -p ~/.config/mise
cp mise/config.toml ~/.config/mise/config.toml
```

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

## Docker Notes

### Linux and WSL

For Docker, use your distro package manager. That usually installs the matching CLI together with the engine.

### macOS

For Docker without Docker Desktop:

```bash
brew install colima docker
colima start
```
