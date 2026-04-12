# Mise Dev Setup

This repo is the portable userland layer for my development environment.

It keeps the OS-specific setup intentionally thin and puts the reusable tooling in `mise`, while Neovim keeps using Mason for editor-only LSPs, formatters, and related tooling.

## Layout

- `mise/config.toml`: copy this to `~/.config/mise/config.toml`
- `nvim/`: optional Neovim config repo or submodule, copied or linked to `~/.config/nvim`

## Terminal First

Get a decent terminal emulator before doing anything else.

- Linux: use `ghostty`
- macOS: use `ghostty`
- Windows: use `wezterm`, then do the actual setup inside WSL

## Host Prerequisites

This repo does not try to manage the OS layer. Install the small set of host utilities Neovim and Mason still need.

Docker is also managed separately from this repo. Install the Docker engine or runtime on the host using the platform-specific notes below.

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

Put your Neovim config repo at `nvim/` in this repo, either as a normal directory or a git submodule. It is intentionally not included here yet.

Then install it to `~/.config/nvim` however you prefer. A plain copy is fine:

```bash
mkdir -p ~/.config
cp -r nvim ~/.config/
```

If you prefer symlinks instead, that is fine too.

Launch Neovim with:

```bash
nvim
```

On first launch:

- `lazy.nvim` clones and installs plugins
- Mason installs LSPs and formatters used only inside Neovim
- Treesitter installs missing parsers

## Why These Tools Live In Mise

The Neovim config needs more than just `nvim` itself.

- `ripgrep` is required for Telescope live grep
- `tree-sitter` is required because the config installs parsers automatically
- `node` is required for npm-backed Mason packages and markdown preview
- `swift` is needed because the config uses `sourcekit` support from the Swift toolchain
- `github-cli` is a useful cross-platform CLI that I want available everywhere
- `odin`, `zig`, `go`, `rust`, `uv`, `bun`, `cmake`, `clang`, `ninja`, and `typst` are general language tooling I want available across machines

LSP binaries and formatters stay in Mason because they are editor-local concerns.

## Platform Notes

### Linux

Do everything directly in your normal user account.

For Docker, use your distro package manager. That usually installs the matching CLI together with the engine.

### macOS

Use the same `mise` config. The only extra host step is getting Command Line Tools installed first.

For Docker without Docker Desktop:

```bash
brew install colima docker
colima start
```

### Windows via WSL

Treat WSL as the real development environment.

- Install `wezterm` on Windows
- Install a Linux distro in WSL
- Install the Linux host prerequisites inside WSL
- Install Docker inside WSL if you want container tooling there
- Install `mise` inside WSL
- Copy this repo's `mise/config.toml` to `~/.config/mise/config.toml` inside WSL
- Keep the Neovim config in the WSL filesystem as well

In that setup, Windows is just providing the terminal window and clipboard.

For a simple Docker setup inside WSL, use the distro package manager in the WSL distro itself.
